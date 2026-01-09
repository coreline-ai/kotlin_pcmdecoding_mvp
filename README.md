# SonicDecoder (kotlin_pcmdecoding_mvp)

[![Project Status: Active](https://img.shields.io/badge/Project%20Status-Active-brightgreen.svg)](https://github.com/HwanChoi/kotlin_pcmdecoding_mvp)
[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)
[![Platform: Android](https://img.shields.io/badge/Platform-Android-orange.svg)](https://developer.android.com/)
[![NDK: Powered](https://img.shields.io/badge/NDK-C++17-red.svg)](https://developer.android.com/ndk)

**SonicDecoder**는 차세대 AI 뮤직 탐색기 'SonicFinder'의 핵심 엔진을 검증하기 위한 초고성능 오디오 분석 및 온디바이스(On-device) LLM 추론 MVP 프로젝트입니다.

---

## 🚀 프로젝트 핵심 가치

본 프로젝트는 단순한 음악 재생기를 넘어, 음악을 **기술적으로 이해하고 의미론적으로 해석**하는 데 집중합니다.

*   **Ultra-Fast Native Engine**: C++ NDK와 하드웨어 가속(AMediaCodec)을 사용하여 수천 곡의 라이브러리를 초고속 분석합니다.
*   **On-device Semantic Intelligence**: 인터넷 연결 없이 기기 내부에서 로컬 LLM을 구동하여 사용자의 자연어 요청을 음악적 태그로 변환합니다.
*   **RAG-based Recommendation**: 음악의 물리적 특징(BPM, 에너지 등)을 6D 벡터화하여 사용자 의도에 가장 가까운 곡을 추천합니다.
*   **Scalable Search**: 대규모 라이브러리를 위해 HNSW ANN + 히스토그램 매퍼로 고속 추천을 지원합니다.

---

## ✨ 주요 기능

### 1. 하드웨어 가속 기반 오디오 분석 (Native Core)
- **High-speed Decoding**: `AMediaExtractor` & `AMediaCodec` 기반 하드웨어 가속 디코딩 파이프라인.
- **Deep Feature Extraction**:
  - **Rhythm**: BPM 추정, BPM Score(추정 신뢰도), Rhythm Stability.
  - **Loudness**: RMS(dBFS), Peak, Crest Factor, Dynamic Range.
  - **Spectral**: Centroid(밝기), Rolloff(개방감), **MFCC Timbre (음색적 특징)**.
  - **Tonal**: Key(조성), Mode(장/단조), Key Strength(조성 강도) 분석.

### 2. 온디바이스 지능형 인터페이스 (AI Engine)
- **Multi-language NLI**: ML Kit 기반 다국어(9개국어+) 언어 식별 및 실시간 번역 지원.
- **LLM Inference**: MediaPipe 기반 **Gemma-2B (int4)** 로컬 구동, 의도 파악 및 16개 상황 태그 매칭.
- **Stability Improvement**: JNI 레이스 컨디션 해결을 위한 Mutex 동기화 적용으로 안정적인 추론 보장.

### 3. 지능형 라이브러리 관리 (Database)
- **Incremental Scan**: MediaStore 변경분만 증분 스캔하여 분석 큐에 반영.
- **Deletion Sync**: 라이브러리 감소 감지 시 FULL 스캔을 강제하여 삭제를 반영.
- **Room SQLite Persistence**: 분석된 모든 오디오 피처와 6D 벡터 데이터를 로컬 DB에 안전하게 보관.

### 4. 검색/추천 엔진 (RAG Search)
- **6D Cosine Similarity**: 감성 벡터 기반 유사도 검색.
- **ANN Index (HNSW)**: 10k+ 라이브러리에서 ANN 자동 전환.
- **Fast Percentile Mapper**: 히스토그램 기반 O(1) 백분위 매핑.
- **MMR Diversity**: 아티스트/앨범 쏠림 방지 리랭킹.
- **Analysis-in-Progress Fallback**: 6D 미완료 트랙은 3D(energy/brightness/bpm) 폴백 검색 지원.

---

## 📊 현재 개발 상태 (Current Status)

현재 프로젝트는 **핵심 기술 파이프라인이 완성**된 단계입니다.

- [x] **NDK 분석 파이프라인**: MFCC, Spectral 피처 및 Tonal 분석 통합 완료.
- [x] **온디바이스 LLM**: 다국어 지원 및 JNI 안정화 완료.
- [x] **6D Mood Mapping**: `Warmth`, `Calm`, `Valence` 등을 포함한 6차원 감성 벡터 엔진 구축 완료.
- [x] **Scalability**: HNSW ANN + 히스토그램 매핑 적용 완료.
- [x] **Library Sync**: MediaStore 증분 스캔 + 삭제 감지(FULL 스캔) 지원.
- [ ] **UI Expansion**: 6D 벡터 시각화 및 고급 믹싱 인터페이스 개발 중.

---

## 🛠 기술 스택

- **Languages**: Kotlin (Android), C++17 (NDK/JNI).
- **Audio Processing**: Android Media NDK, FFTW-like DSP logic.
- **AI/ML**: MediaPipe LLM Inference, Google ML Kit (Translation/ID).
- **Architecture**: MVVM(LLM MVP), UseCase/Repository, WorkManager, Room Persistence Library.

---

## 🏃 시작하기

### 모델 파일 준비
MediaPipe에서 지원하는 `gemma-2b-it-cpu-int4.bin` 또는 호환 모델이 필요합니다.
```powershell
# 모델 파일을 기기의 임시 폴더로 전송
adb push gemma-2b-it-cpu-int4.bin /data/local/tmp/
```

### 설치 및 로드
1. 저장소를 클론합니다.
2. Android Studio에서 프로젝트를 엽니다.
3. Gradle 빌드를 실행합니다.
4. 앱 실행 후 **LLM MVP** 탭에서 **'전체 분석 다시 실행 (DB 리셋)'** 버튼으로 라이브러리를 재분석할 수 있습니다.

### CLI 명령 (ADB)
```powershell
.\gradlew.bat :app:assembleDebug
adb install -r app\build\outputs\apk\debug\app-debug.apk
adb shell am start -n com.example.sonicdecodertest/.MainActivity
adb logcat -s SonicDecoderTest:I
```

---

## 📈 향후 로드맵 (Roadmap)

1.  **Visual Interaction**: 6차원 감성 벡터를 직접 조절할 수 있는 다차원 슬라이더 UI 구현.
2.  **Library Source 확장**: SAF 폴더 import + 길이 미확보 FD fallback 강화.
3.  **Feature Upgrade**: onset/flux, band ratios, flatness, LUFS/LRA 등 추가.
4.  **Schema 정합**: raw_json v1.1 스펙 정합 및 확장.
5.  **Cross-platform Core**: 분석 엔진(C++)의 재사용성을 극대화하여 멀티 플랫폼 확장 준비.

---

## 📄 라이선스

본 프로젝트는 **GNU Affero General Public License v3.0 (AGPL-3.0)**에 따라 배포됩니다.

---
*Developed by Hwan Choi for SonicFinder Project.*
