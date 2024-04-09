# 로그아웃

## 로그아웃 기능 구현

- API 호출해서 무효화 처리를 통해 로그아웃
  - 무효화하는 처리는 백엔드에서 진행(API 요청까지만)
  - 로그아웃 버튼 클릭 후 localStorage 초기화
  - 홈(/)으로 리다이렉션

<br/>

### API 메서드 추가

- src/services/ApiService.ts

```ts
async logout(): Promise<void> {
  await this.instance.delete('/session');
}
```

<br/>

### 로그아웃 버튼이 있는 `Header` 컴포넌트 수정

```jsx
export default function Header() {
  const navigate = useNavigate();

  const { accessToken, setAccessToken } = useAccessToken();

  const { categories } = useFetchCategories();

  const handleClickLogout = async () => {
    await apiService.logout();
    setAccessToken('');
    navigate('/');
  };

  // …(중략)…
}
```

### ✅ Refactor

- react-router-dom를 Mocking `Header` 컴포넌트 테스트
