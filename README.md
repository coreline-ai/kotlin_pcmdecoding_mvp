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
- **Multi-language NLI**: ML Kit 기반 다국어(8개국어+) 언어 식별 및 실시간 번역 지원.
- **LLM Inference**: MediaPipe 기반 **Gemma-2B (int4)** 로컬 구동, 의도 파악 및 16개 상황 태그 매칭.
- **Stability Improvement**: JNI 레이스 컨디션 해결을 위한 Mutex 동기화 적용으로 안정적인 추론 보장.

### 3. 지능형 라이브러리 관리 (Database)
- **Incremental Analysis**: 파일 시그니처 감지를 통해 변경된 곡만 선택적으로 분석하는 효율적인 인덱싱.
- **Room SQLite Persistence**: 분석된 모든 오디오 피처와 6D 벡터 데이터를 로컬 DB에 안전하게 보관.

---

## 📊 현재 개발 상태 (Current Status)

현재 프로젝트는 **핵심 기술 파이프라인이 90% 이상 완성**된 단계입니다.

- [x] **NDK 분석 파이프라인**: MFCC, Spectral 피처 및 Tonal 분석 통합 완료.
- [x] **온디바이스 LLM**: 다국어 지원 및 JNI 안정화 완료.
- [x] **6D Mood Mapping**: `Warmth`, `Calm`, `Valence` 등을 포함한 6차원 감성 벡터 엔진 구축 완료.
- [/] **Scalability**: 대용량 라이브러리(10k+) 대응을 위한 HNSW 인덱싱 도입 준비 중.
- [/] **UI Expansion**: 6D 벡터 시각화 및 고급 믹싱 인터페이스 개발 중.

---

## 🛠 기술 스택

- **Languages**: Kotlin (Android), C++17 (NDK/JNI).
- **Audio Processing**: Android Media NDK, FFTW-like DSP logic.
- **AI/ML**: MediaPipe LLM Inference, Google ML Kit (Translation/ID).
- **Architecture**: MVVM, Jetpack WorkManager, Room Persistence Library.

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
4. 앱 실행 시 **'Analyze All'** 버튼을 통해 기기 내 음악을 기술적으로 분석할 수 있습니다.

---

## 📈 향후 로드맵 (Roadmap)

1.  **ANN Indexing**: 1만 곡 이상의 대규모 라이브러리에서 실시간 벡터 검색이 가능한 ANN 엔진 도입.
2.  **Visual Interaction**: 6차원 감성 벡터를 직접 조절할 수 있는 다차원 슬라이더 UI 구현.
3.  **Cross-platform Core**: 분석 엔진(C++)의 재사용성을 극대화하여 멀티 플랫폼 확장 준비.

---

## 📄 라이선스

본 프로젝트는 **GNU Affero General Public License v3.0 (AGPL-3.0)**에 따라 배포됩니다.

---
*Developed by Hwan Choi for SonicFinder Project.*
