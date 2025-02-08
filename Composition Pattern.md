# Composition Pattern
조합 패턴(Composition Pattern)은 작은 컴포넌트들을 조합하여 더 큰 컴포넌트를 만드는 패턴이다. 

- **재사용성 증가**: 작은 컴포넌트를 여러 곳에서 재사용 가능
- **유연성 확보**: 다양한 기능을 유연하게 조합 가능
- **상속보다 더 직관적**: React는 상속 대신 조합을 권장

### 조합 패턴의 예시

###### `props.children`을 활용한 조합
```tsx
// Card.js (조합 가능한 부모 컴포넌트)
function Card({ children }) {
  return (
    <div style={{ border: '1px solid gray', padding: '20px' }}>
      {children}
    </div>
  );
}

// App.js
export default function App() {
  return (
    <Card>
      <h2>타이틀</h2>
      <p>이것은 카드 내부의 콘텐츠입니다.</p>
    </Card>
  );
}
```

###### 컴포넌트 조합 (Slot Pattern)
```tsx
// Layout.js
function Layout({ header, content, footer }) {
  return (
    <div>
      <header>{header}</header>
      <main>{content}</main>
      <footer>{footer}</footer>
    </div>
  );
}

// App.js
export default function App() {
  return (
    <Layout
      header={<h1>헤더입니다</h1>}
      content={<p>이곳은 본문 내용입니다.</p>}
      footer={<small>푸터 영역입니다</small>}
    />
  );
}

```


### 조합 패턴 vs 상속

| **조합 패턴 (Composition)** | **상속 (Inheritance)** |
| ----------------------- | -------------------- |
| 작은 컴포넌트를 유연하게 조합 가능     | 강한 결합으로 코드 유연성이 낮음   |
| 재사용성과 확장성이 높음           | 재사용성이 제한적임           |
| React의 권장 방식            | React에서 권장하지 않음      |
