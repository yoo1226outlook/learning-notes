## Summary

브라우저가 보안을 위해 다른 출처(Origin) 간의 리소스 요청을 제한하는 정책과 이를 안전하게 허용하기 위한 메커니즘입니다. 프론트엔드(React)와 백엔드(Node.js)가 서로 다른 도메인이나 포트에서 실행될 때 반드시 설정해야 하는 보안 검문소 역할을 합니다.

## Key Concepts

- **Origin**: 프로토콜(http/https), 도메인, 포트번호의 조합을 의미하며, 이 중 하나라도 다르면 "교차 출처"로 간주됩니다.
    
- **Preflight Request**: 실제 요청을 보내기 전, 브라우저가 `OPTIONS` 메서드를 통해 서버가 해당 요청을 허용하는지 미리 확인하는 예비 통신입니다.
    
- **Access-Control-Allow-Origin**: 서버가 응답 헤더에 명시하는 필드로, 어떤 웹사이트가 자사의 리소스에 접근할 수 있는지 결정합니다.
    
- **Credentials**: 쿠키나 인증 헤더를 포함한 통신을 의미하며, 서버와 클라이언트 양쪽에서 명시적인 허용 설정(`credentials: true`)이 필요합니다.
    

## Example

### Node.js (Express)에서 CORS 설정 적용

```javascript
import cors from "cors";

const app = express();

app.use(cors({
  origin: "[https://secret-journal.onrender.com](https://secret-journal.onrender.com)", // 허용할 클라이언트 주소
  credentials: true, // 세션 쿠키 공유 허용
  methods: ["GET", "POST", "PATCH", "DELETE"] // 허용할 HTTP 메서드
}));
```

## Notes

- **보안 주의**: `origin: "*"`(와일드카드)는 모든 사이트의 접근을 허용하므로 보안에 취약하며, `credentials: true`와 함께 사용할 수 없습니다.
    
- **배포 환경**: 로컬(`localhost`)과 운영 서버의 주소가 다르므로, `ORIGIN_URL`과 같은 환경 변수를 사용하여 유연하게 관리해야 합니다.
    
- **에러 로그**: 브라우저 콘솔에 "CORS error"가 뜬다면, 서버의 `origin` 설정값과 클라이언트의 실제 접속 주소가 토씨 하나 안 틀리고 일치하는지 확인해야 합니다 (슬래시 `/` 유무 포함).
    

## Related

- [[Express Middleware]]
    
- [[HTTP Headers]]
    
- [[Preflight Request]]
    
- [[Same-Origin Policy]]
    
- [[Vite Environment Variables]]