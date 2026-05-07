# 잉크 메모장 - Vercel 배포 가이드

Firebase + Vercel로 배포하는 메모장 앱

## 🚀 배포 방법

### 방법 1: Vercel CLI (추천)

1. **Vercel CLI 설치**
   ```bash
   npm install -g vercel
   ```

2. **프로젝트 폴더에서 배포**
   ```bash
   vercel
   ```

3. **질문에 답변**
   - Set up and deploy? → Y
   - Which scope? → 본인 계정 선택
   - Link to existing project? → N
   - Project name? → memo-app (또는 원하는 이름)
   - In which directory is your code located? → ./

4. **배포 완료!**
   - URL이 표시됩니다 (예: https://memo-app-xxx.vercel.app)

### 방법 2: GitHub + Vercel 연동

1. **GitHub 저장소 생성**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/사용자명/저장소명.git
   git push -u origin main
   ```

2. **Vercel 대시보드에서 Import**
   - https://vercel.com 접속
   - "Add New" → "Project"
   - GitHub 저장소 선택
   - "Deploy" 클릭

3. **자동 배포 설정 완료**
   - 코드 푸시할 때마다 자동 재배포

## 📋 파일 구조

```
프로젝트/
├── memo.html       # 메인 앱 (Firebase 설정 완료)
├── vercel.json     # Vercel 설정
├── .gitignore      # Git 무시 파일
└── README.md       # 이 파일
```

## 🔥 Firestore 보안 규칙 설정

배포 전에 Firebase Console에서 보안 규칙을 설정하세요:

**Firebase Console → Firestore Database → 규칙**

### 개발/테스트용 (공개):
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

### 프로덕션용 (인증 필요):
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /memos/{memoId} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

## ✅ 배포 후 확인사항

1. ✅ Firestore 연결 상태 (화면 우측 상단 동기화 상태 확인)
2. ✅ 메모 작성/삭제 테스트
3. ✅ 다른 기기에서 실시간 동기화 확인

## 🛠️ 문제 해결

### "Firebase 설정 필요" 오류
→ memo.html의 firebaseConfig가 제대로 입력되었는지 확인

### "연결 오류" 표시
→ Firebase Console → Firestore → 규칙에서 읽기 권한 확인

### Vercel 배포 실패
→ vercel.json 파일이 있는지 확인

## 📞 지원

문제가 있으면 Vercel/Firebase 문서를 참고하세요:
- Vercel: https://vercel.com/docs
- Firebase: https://firebase.google.com/docs
