## Summary

Express 환경에서 `multipart/form-data` 형식을 처리하여 파일 업로드를 가능하게 해주는 미들웨어입니다. 주로 이미지나 문서와 같은 바이너리 데이터를 서버의 특정 폴더에 저장하거나 외부 스토리지로 전달할 때 사용됩니다.

## Key Concepts

- **diskStorage**: 서버 로컬 디스크에 파일을 저장하기 위한 엔진으로, 저장 위치와 파일 이름을 세밀하게 제어할 수 있습니다.
    
- **destination**: 업로드된 파일이 물리적으로 저장될 경로를 지정합니다.
    
- **filename**: 서버 내 파일명 중복을 방지하기 위해 `Date.now()`나 랜덤 값을 조합하여 고유한 이름을 생성합니다.
    
- **Middleware Integration**: 특정 라우터에 `upload.single('fieldName')` 형태로 삽입되어 요청 본문보다 파일을 먼저 처리합니다.
    

## Example

### Multer 설정 및 라우터 적용

``` javascript
import multer from "multer";
import path from "path";

// 1. 저장 엔진 설정
const storage = multer.diskStorage({
    destination: (req, file, cb) => cb(null, "uploads/"),
    filename: (req, file, cb) => {
        const uniqueSuffix = Date.now() + "-" + Math.round(Math.random() * 1e9);
        cb(null, uniqueSuffix + path.extname(file.originalname));
    }
});

// 2. 업로드 미들웨어 생성
const upload = multer({ storage: storage });

// 3. 라우터에 적용 (req.file로 데이터 접근)
app.post("/api/upload", upload.single("image"), (req, res) => {
    res.json({ url: `/uploads/${req.file.filename}` });
});
```

## Notes

- **보안**: 업로드 폴더(`uploads/`)는 `.gitignore`에 추가하여 실제 파일이 저장소에 올라가지 않도록 관리해야 합니다.
    
- **순서 주의**: `multer` 변수 정의와 설정 코드는 반드시 해당 미들웨어를 사용하는 라우터 핸들러보다 상단에 위치해야 `ReferenceError`를 방지할 수 있습니다.
    
- **정적 서빙**: 업로드된 이미지를 브라우저에서 보려면 `app.use("/uploads", express.static("uploads"))` 설정이 병행되어야 합니다.
    

## Related

- [[Express Middleware]]
    
- [[FormData]]
    
- [[Static Serving]]
    
- [[fs.unlink]]