# 3. 배송 및 결제 정보 전달

## 1. 배송 및 결제 정보 전달

🎯 백엔드로 정보 전달하기 

정보를 전달한 후에 일어나는 일은 백엔드에서 처리하기 때문에 여기서는 다루지 않음  

### `ApiService`에 createOder 메소드 추가

```tsx
async createOrder({ receiver, payment }: {
  receiver: {
    name: string;
    address1: string;
    address2: string;
    postalCode: string;
    phoneNumber: string;
  };
  payment: {
    merchantId: string;
    transactionId: string;
  };
}): Promise<void> {
  await this.instance.post('/orders', { receiver, payment });
}
```

* 순서는 상관 없음
* 흐름상 카트 처리하는 로직 뒤에 붙이면 괜찮음  

### `OrderFormStore`에 order 메소드 추가

* 로그인, 회원가입 로직과 유사 

```tsx
async order({ merchantId, transactionId }: {
  merchantId: string;
  transactionId: string;
}) {
  apiService.createOrder({
    receiver: {
      name: this.name,
      address1: this.address1,
      address2: this.address2,
      postalCode: this.postalCode,
      phoneNumber: this.phoneNumber,
    },
    payment: { merchantId, transactionId },
  });
}
```

### PaymentButton 컴포넌트에서 추가 작업

useOrderFormStore 에서 store 얻어오기  

#### 실패하는 경우

1. 사용자가 중단(창 끄기)
2. 잔액 부족, 한도 초과

<br>

## 2. 다양한 기능 

백엔드와 협업이 자유롭고 사고를 넓히면 서비스를 고도화시킬 수 있음  
전체적인 내용을 알아야 백엔드와도 소통할 수 있음  
💡 포트원이 아니더라도 비슷한 형태로 처리되기 때문에 프로세스를 잘 익혀두면 좋음   

* 백엔드와 주소록 관리 기능 만들어서 처리하기
* 장바구니 관리를 프론트에서 처리하기 (Ex. 배민 장바구니)
* 쿠폰 적용 기능, 쿠폰 적용가 계산하기  
