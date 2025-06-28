
# 🛠️ Pintos Docker 개발 환경 공유 가이드

이 문서는 팀 프로젝트에서 Pintos 개발 환경을 동일하게 맞추기 위해 Docker를 이용한 이미지 공유 및 실행 방법을 정리한 가이드입니다.

---

## ✅ 전체 진행 요약

1. 팀원1이 Docker 컨테이너에서 개발 환경을 구성한 뒤, 이미지로 저장 (`commit`)
2. 팀원2는 이미지 파일을 받아서 로컬에 복원 (`load`)
3. 컨테이너 실행 시, 각자 로컬의 소스코드를 `/workspace`에 마운트하여 작업
4. VS Code에서 로컬 폴더를 열고, Docker 터미널에서 컴파일 및 실행

---

## 📦 1. 환경 구성 및 이미지 저장 (팀장 측 작업)

### 🔹 현재 컨테이너를 이미지로 저장

```bash
docker commit pintos-dev pintos-image:v1
````

### 🔹 이미지 파일로 저장 (.tar)

```bash
docker save pintos-image:v1 -o pintos-image-v1.tar
```

* `pintos-dev`: 현재 사용 중인 컨테이터 이름
* `pintos-image:v1`: 저장할 이미지 이름:태그
* `pintos-image-v1.tar`: 다른 팀원에게 전달할 파일 (USB, 구글 드라이브 등)

---

## 🔄 2. 이미지 복원 및 컨테이너 실행 (팀원 측 작업)

### 🔹 이미지 불러오기

```bash
docker load -i pintos-image-v1.tar
```

정상적으로 로드되면 다음과 같이 표시됩니다:

```
Loaded image: pintos-image:v1
```

### 🔹 이미지 목록 확인

```bash
docker images
```

---

## 🐳 3. 컨테이너 실행 (로컬 코드 마운트 포함)

```bash
docker run --platform linux/amd64 -it --name pintos-dev \
  -v ~/Desktop/pintos:/workspace \
  -w /workspace \
  pintos-image:v1
```
**주의 사항 : ~/Desktop/pintos 부분은 자신의 pintos 코드 주소로 할 것!**

* `--platform linux/amd64`: M1/M2 맥북/arm 환경에서 x86 환경으로 실행
* `-v`: 로컬 소스코드 디렉토리를 `/workspace`로 바인드 마운트
* `-w`: 시작 시 기본 디렉토리 설정
* `pintos-image:v1`: 공유받은 Docker 이미지

---

## 💻 4. VS Code에서 작업하기

### 🔹 소스코드 편집

* VS Code에서 로컬 폴더(`~/Desktop/pintos`)를 열고 코드 수정

### 🔹 컴파일 및 실행은 Docker 컨테이너 내부에서 진행

컨테이너 접속:

```bash
docker exec -it pintos-dev bash
```
