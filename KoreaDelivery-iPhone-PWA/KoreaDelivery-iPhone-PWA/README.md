# 고려 딜리버리 아이폰 무료 웹앱 (PWA)

## 설치 / 무료 공개 주소 만들기 (GitHub Pages)
1. GitHub 로그인 → New repository → 예: `korea-rider-gps` → Private 대신 **Public** 선택 (GitHub Pages 무료 설정에 맞게 선택). 위치 데이터나 비밀키를 파일에 포함하지 마세요.
2. 압축 해제한 **이 폴더 안의 파일 전체** (`index.html`, `app.js`, `sw.js`, `manifest.webmanifest`, `icons/`)를 저장소 루트에 업로드합니다.
3. Settings → Pages → Build and deployment → Deploy from a branch → `main` / `(root)` → Save.
4. 공개된 `https://사용자명.github.io/korea-rider-gps/` 주소를 아이폰 Safari로 엽니다. 실제 URL은 GitHub Pages 설정 화면에 표시됩니다.
5. Safari 공유 버튼 → **홈 화면에 추가** → 추가.
6. 앱 열기 → 서버 연결 설정 → 기존 Supabase **publishable/anon 키**와 기사 ID 입력 → 저장 → 운행 시작 → 위치 권한 허용.
7. 관리자 지도에서 동일한 `rider_id`의 위도/경도 및 `last_seen_at` 갱신을 확인합니다.

## 중요한 제한
- iOS Safari/PWA는 화면 잠금, 앱 전환, OS 백그라운드 제어 시 위치 추적이 멈출 수 있습니다. 24시간 위치 공유용으로 사용하지 마세요.
- 현재 기존 안드로이드 테스트 앱과 동일하게 `rider_locations` 행에 PATCH 합니다. **해당 기사 ID의 행이 미리 존재**해야 하며, UPDATE 권한과 RLS 정책이 허용해야 합니다.
- 이 샘플은 **로그인/인증이 없는 테스트용**입니다. 기사별 권한 제어가 없으면 다른 사람의 위치 데이터 변조 또는 노출 위험이 있습니다. 운영 배포 전 Supabase Auth + 기사별 RLS 정책을 구현하세요.
- 서비스 역할 키(`service_role`, `sb_secret_...`)는 절대 브라우저에 넣지 마세요. 공개키도 사용자의 기기에 저장되므로 RLS가 반드시 필요합니다.
- 서버에서 변경된 행이 0개면 전송 실패로 표시됩니다.
- 앱을 종료하거나 iOS가 웹앱을 중단시키면 서버에 종료 상태를 전송할 수 없습니다. 관리자 지도는 `last_seen_at`을 기준으로 오래된 위치를 오프라인으로 판단해야 합니다.
- HTTPS 환경에서만 위치 권한이 작동합니다.

## 로컬 테스트
윈도우 VS Code Live Server 또는 `python -m http.server 8000`으로 PC에서 열 수 있지만, 아이폰에서는 반드시 HTTPS로 배포한 주소를 사용하세요.
