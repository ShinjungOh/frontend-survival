# 1. 배송 정보 입력

## 1. 기능

> 🎯 어떤 순서로 기능을 만들 것인지는 비즈니스 우선 순위에 따라 결정

1. 상품 목록 확인 ✅
2. 상품 상세 정보 확인 ✅
3. 장바구니에 상품 담기 ✅
4. 로그인 ✅
5. 로그아웃 ✅
6. 회원 가입 ✅
7. 주문 목록 확인 ✅
8. 주문 상세 확인 ✅
9. 주문하기 → 배송지 입력, 결제

<br>

## 2. 화면

1. 홈 페이지 - `/`
2. 상품 목록 페이지 - `/products`
3. 상품 상세 페이지 - `/products/{id}`
4. 장바구니 페이지 - `/cart`
5. 로그인 페이지 - `/login`
6. 회원 가입 페이지 - `/signup`
7. 회원 가입 완료 페이지 - `/signup/complete`
8. 주문 목록 페이지 - `/orders`
9. 주문 상세 페이지 - `/orders/{id}`
10. 주문 완료 페이지 - `/order/complete`
11. 주문 페이지 - `/order`

<br>

## 3. 주문 페이지

🎯 주문 페이지와 주문 완료 페이지를 추가해서 전체 흐름을 만들기 

### `OrderPage` 생성

로직 자체는 장바구니와 매우 유사함

### `OrderForm` 컴포넌트 생성

역시 장바구니와 유사

* 주소에 `/order`를 강제로 입력해서 들어오는 것은 고려하지 않음
* 결제는 솔루션을 이용(추후 작업)

```tsx
const Container = styled.div`
  h2 {
    font-size: 4rem;
  }
`;

type OrderFormProps = {
  cart: Cart;
};

export default function OrderForm({ cart }: OrderFormProps) {
  return (
    <Container>
      <h2>주문</h2>
      <Table
        lineItems={cart.lineItems}
        totalPrice={cart.totalPrice}
      />
      {/* TODO: 배송지 입력 */}
      {/* TODO: 결제 */}
    </Container>
  );
}
```

### `OrderCompletePage` 생성 

`SignupCompletePage`와 마찬가지로 간단하게 구성

```tsx
export default function OrderCompletePage() {
  return (
    <div>
        <p>주문이 완료되었습니다.</p>
        <p><Link to="/orders">주문 목록 확인</Link></p>
    </div>
  );
}
```

### routes에 페이지 추가

```
{ path: '/order', element: <OrderPage /> },
{ path: '/order/complete', element: <OrderCompletePage /> },
```

### `CartView`에 버튼 추가

장바구니 페이지에서 주문 페이지로 갈 수 있도록 버튼 추가  

```tsx
export default function CartView({ cart }: CartViewProps) {
  const navigate = useNavigate();

  const handleClick = () => {
    navigate('/order');
  };

  return (
    <div>
      {/**/}
      <Button onClick={handleClick}>주문하기</Button>
    </div>
  );
}
```

<br>

## 4. 배송지 정보 입력

🎯 **받는 사람 이름, 주소, 전화번호**를 입력 받는 폼 구현하기 

* `OrderFormStore`를 만들어서 관련 정보를 다룰 수 있도록 준비   
* 주소는 크게 두 부분으로 나뉨 - 상위 주소, 상세 주소 
* **상세 주소**가 아닌 **상위 주소**를 지정할 때 우편번호를 함께 지정 

### OrderFormStore Hook 생성 

```tsx
@singleton()
@Store()
export default class OrderFormStore {
  name = '';

  address1 = '';

  address2 = '';

  postalCode = '';

  phoneNumber = '';

  get valid() {
    return !!this.name.trim()
      && !!this.address1.trim()
      && !!this.address2.trim()
      && !!this.postalCode.trim()
      && !!this.phoneNumber.trim();
  }

  @Action()
  changeName(name: string) {
    this.name = name;
  }

  @Action()
  changeAddress1(address1: string, postalCode: string) {
    this.address1 = address1;
    this.postalCode = postalCode;
  }

  @Action()
  changeAddress2(address2: string) {
    this.address2 = address2;
  }

  @Action()
  changePhoneNumber(phoneNumber: string) {
    this.phoneNumber = phoneNumber.replace(/[^0-9]/g, '');
  }
}
```

* 지번/도로명 주소가 우편번호와 매칭되기 때문에 동시에 사용
* 전화번호는 숫자만 가져오도록 정규표현식 사용 

### useOrderFormStore Hook 생성 

* src/hooks 
* Store를 쓸 수 있는 Hook 생성  

### `OrderForm`에서 쓸 수 있는 `ShippingForm` 생성 

```tsx
const Container = styled.div`
  h3 {
    font-size: 2rem;
  }

  input {
    width: 50rem;
  }
`;

const PostalCodeField = styled.div`
  > div {
    display: inline-block;
    margin-right: 1rem;
  }

  input {
    width: 10rem;
  }
`;

export default function ShippingForm() {
  const [{
    name, address1, address2, postalCode, phoneNumber,
  }, store] = useOrderFormStore();

  const handleClickSearchPostalCode = () => {
    // TODO: 우편번호 검색
  };

  const handleChangeName = (value: string) => {
    store.changeName(value);
  };

  // TODO: 우편번호 검색할 때 사용  
  const handleChangeAddress1 = (value: {
    address: string;
    postalCode: string;
  }) => {
    store.changeAddress1(value.address, value.postalCode);
  };

  const handleChangeAddress2 = (value: string) => {
    store.changeAddress2(value);
  };

  const handlePhonNumber = (value: string) => {
    store.changePhoneNumber(value);
  };

  return (
    <Container>
      <h3>받는 사람</h3>
      <TextBox
        label="이름"
        placeholder="받는 분 이름"
        value={name}
        onChange={handleChangeName}
      />
      <PostalCodeField>
        <TextBox
          label="우편번호"
          value={postalCode}
          readOnly
        />
        <Button onClick={handleClickSearchPostalCode}>
          우편번호 검색
        </Button>
      </PostalCodeField>
      <TextBox
        label="주소"
        value={address1}
        readOnly
      />
      <TextBox
        label="상세 주소"
        value={address2}
        onChange={handleChangeAddress2}
      />
      <TextBox
        label="전화번호"
        type="tel"
        value={phoneNumber}
        onChange={handlePhonNumber}
      />
    </Container>
  );
}
```

### `TextBox`에 readOnly 속성 추가

readOnly 속성을 추가하고 onChange를 옵셔널하게 만들기

* `OrderForm.tsx`의 **TODO: 배송지 입력**에 `<ShippingForm />` 추가  
* type에 tel 추가 

```tsx
type TextBoxProps = {
  label: string;
  placeholder?: string;
  type?: 'text' | 'number' | 'password' | 'tel'; // ...and more types...
  value: string;
  onChange?: (value: string) => void;
  readOnly?: boolean;
}

export default function TextBox({
  label, placeholder = undefined, type = 'text', value,
  onChange = undefined, readOnly = false,
}: TextBoxProps) {
  const id = useRef(`textbox-${Math.random().toString().slice(2)}`);

  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    if (!onChange) {
      return;
    }
    onChange(event.target.value);
  };

  return (
    <Container>
      <label htmlFor={id.current}>
        {label}
      </label>
      <input
        id={id.current}
        type={type}
        placeholder={placeholder}
        value={value}
        onChange={handleChange}
        readOnly={readOnly}
      />
    </Container>
  );
}
```

<br>

## 5. 우편번호 검색

### 우편번호 검색 기능

1. 주소는 외우지만 우편번호는 못 외우는 경우
2. 정확히 표준화된 주소를 얻을 수 있음 ⭐️ 

[Daum 우편번호 서비스](https://postcode.map.daum.net/guide)

* key 발급 필요 없고 그냥 사용하면 됨
* 매우 편리 
* 여기에서는 ssl 방식으로 처리할 것  


### `index.html` 파일에 스크립트 태그 추가

⚠️ script의 순서에 주의할 것  
로딩이 먼저 완료될 수 있도록 위에 작성하기  

```html
<script src="https://t1.daumcdn.net/mapjsapi/bundle/postcode/prod/postcode.v2.js"></script>
<script type="module" src="src/main.tsx"></script>
```

### 타입 지정 

전역으로 얻는 daum 객체의 타입을 알 수 있도록 `daum.postcode.d.ts` 파일 생성

* components/new-order

```tsx
declare namespace daum {
  export type PostcodeResult = {
    address: string;
    zonecode: string;
  }

  export class Postcode {
    constructor({ oncomplete, width, height }: {
      oncomplete: (data: PostcodeResult) => void;
      width: string;
      height: string;
    });

    embed(element: HTMLElement | null);
  }
}
```

* 팝업 모달로 사용할 경우 open 으로 바꾸면 됨 

### `AddressSearch` 컴포넌트 생성 

우편번호 검색 모달(modal)을 보여주는 `AddressSearch` 컴포넌트 만들기  

useRef 훅을 사용하면 DOM 객체에 직접 접근할 수 있음

* 내용이 바뀌어도 리렌더링 되지 않음 
* API 사용 시 이런 방법으로 구현하면 됨  

```tsx
const Container = styled.div`
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, .5);

  div {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 80%;
    height: 80%;
    max-width: 50rem;
    background: ${(props) => props.theme.colors.background};
  }
`;

type AddressSearchProps = {
  close: ()=> void;
  changeAddress: ({ address, postalCode }: {
    address: string;
    postalCode: string;
  }) => void;
}

export default function AddressSearch({
  close, changeAddress,
}: AddressSearchProps) {
  const refElement = useRef<HTMLDivElement>(null);

  useEffectOnce(() => {
    new daum.Postcode({
      oncomplete(data) {
        const { address, zonecode: postalCode } = data;
        changeAddress({ address, postalCode });
        close();
      },
      width: '100%',
      height: '100%',
    }).embed(refElement.current);
  });

  return (
    <Container id="address-search-container" onClick={close}>
      <div ref={refElement} />
    </Container>
  );
}
```

* 폼의 바깥 부분을 누르면 close 

### `ShippingForm`에서 `AddressSearch`를 활용하도록 구현 

```tsx
const {
  value: searching, setTrue: openSearch, setFalse: closeSearch,
} = useBoolean();

// …(중략)…

const handleClickSearchPostalCode = () => {
  openSearch();
};

// …(중략)…

{searching && (
  <AddressSearch
    close={closeSearch}
    changeAddress={handleChangeAddress1}
  />
)}
```

### codecept를 이용한 팁

개발할 때 매번 배송 정보 입력 폼을 임의로 작성하는 것이 번거로움   
💡 E2E 테스트로 모든 input을 채워넣고 마지막에 wait로 대기 시간을 길게 주면 
그동안 페이지에서 개발자 도구를 열어 디버깅 하는 등의 행위를 할 수 있음   
