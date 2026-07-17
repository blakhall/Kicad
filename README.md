# 📝 사내 KiCad 공통 라이브러리 관리 규정 (README)

본 문서는 사내 KiCad(키캐드) 심볼, 풋프린트, 3D 모델 라이브러리의 분류 체계를 표준화하고, 다른 부서 및 외부 참조 라이브러리와 유기적으로 동기화하여 관리하기 위한 표준 규정입니다.

---

## 1. 외부 참조 라이브러리 연동 및 유기적 관리 규정

사내 하드웨어 설계의 통일성과 검증된 회로 자산의 재사용을 위해, 아래의 외부 프로젝트 참조 폴더 내 파일과 이 저장소를 유기적으로 연동하여 관리합니다.

* **연동 및 참조 대상 폴더 경로:**  
  `D:\huni\sw\antigravity\k_dms\storage\archive\Projects\PMO\03_Ref\KiCad`
* **유기적 연동 및 관리 원칙:**
  1. **환경 변수 준수 (`KICAD_GITHUB`):**  
     참조 폴더의 가이드 문서(`KiCad_환경변수_경로설정_가이드.md`)에 따라, 프로젝트 내부 경로 지정 시 특정 개인의 절대 경로 대신 환경 변수 **`${KICAD_GITHUB}`**를 사용하여 치환합니다.
     * 모든 팀원은 본인의 KiCad 경로 구성(Configure Paths)에서 `KICAD_GITHUB` 변수명에 로컬 저장소 경로(`D:\huni\data\github\Kicad`)를 정확히 매핑하여 사용해야 합니다.
  2. **검증 자산의 동기화:**  
     `k_dms` 내의 PMO 관련 검증용 공통 라이브러리 자산에 업데이트가 발생할 경우, 이 저장소의 공통(`common`) 및 범용(`_gen`) 라이브러리에도 즉시 동기화하여 버전의 일관성을 유지합니다.
  3. **회로도 및 풋프린트 경로 치환 규칙:**  
     프로젝트 라이브러리 테이블(`sym-lib-table`, `fp-lib-table`) 등록 시, 환경 변수를 활용하여 아래 예시와 같이 유기적인 상대 경로 매핑을 구현합니다.
     * *올바른 예 (환경 변수 치환):* `(uri "${KICAD_GITHUB}/symbols/common_gen.kicad_sym")`
     * *잘못된 예 (절대 경로 사용 금지):* `(uri "D:/huni/data/github/Kicad/symbols/common_gen.kicad_sym")`

---

## 2. 라이브러리 분류 및 명명 표준 (Library Grouping Rules)

심볼(`symbols/`) 및 풋프린트(`footprints/`) 라이브러리는 무분별한 파일 생성을 방지하고 탐색 효율성을 극대화하기 위해 다음 규칙에 따라 관리합니다.

1. **공통 기본 소자 (`common` 계열):**
   * 저항(R), 콘덴서(C), 인덕터(L), 다이오드, GND 등 모든 보드에서 기본적으로 재사용되는 패시브 및 전원 기본 기호는 `common.kicad_sym` 및 `common.pretty`에만 수록합니다.
2. **기능별 범용 IC 및 소자 (`*_gen` 계열):**
   * 특정 단일 부품이 아니며 광범위하게 쓰이는 기능별 액티브 칩셋들은 접미사 `_gen`이 붙은 카테고리 파일에 병합하여 관리합니다.
   * *예시:* `mcu_gen.kicad_sym` (범용 MCU), `power_gen.kicad_sym` (범용 전원 IC 및 MOSFET), `common_gen.kicad_sym` (범용 아날로그/기초 IC)
3. **단독 부품 라이브러리 파일 생성 지양:**
   * 특정 단일 칩을 위한 개별 라이브러리 파일(예: `CSD17308.kicad_sym` 등)은 무분별한 파일 개수 증가를 유발하므로 원칙적으로 생성을 금지합니다.
   * 단, 핀 수가 100개 이상이거나 구조가 매우 독특한 대형 파워 모듈이나 특정 통신 모듈 등의 특수 부품에 한해서만 독자적인 명명을 예외 허용합니다.

---


---

## 3. 심볼 파라메터 필드 규정 (Symbol Property Fields Rule)

모든 부품 심볼에는 회로도 설계 완료 후 BOM(Bill of Materials) 추출 및 자재 매칭을 위해 아래의 **표준 파라메터 속성 필드**를 필수로 추가해야 합니다.

1. **`Reference` (참조 지정자):**  
   * 부품 카테고리에 맞는 영문 지시자 (예: IC는 `U`, 저항은 `R`, 콘덴서는 `C` 등). 심볼 상단에 표시 (글자 크기 1.27mm).
2. **`Value` (부품 값 / 모델명):**  
   * 실제 부품의 핵심 값 또는 세부 모델명 (예: `10k`, `0.1uF`, `MCP2122T-E/SN`). 심볼 하단에 표시 (글자 크기 1.27mm).
3. **`Footprint` (풋프린트):**  
   * 해당 부품의 PCB 패드 패턴 경로. 필수 지정하되 회로도상에서는 **숨김(Hide)** 처리.
4. **`Datasheet` (데이터시트):**  
   * 해당 부품의 제조사 공식 데이터시트 웹 링크. 필수 지정하되 **숨김(Hide)** 처리.
5. **`Description` (사양 설명):**  
   * 부품의 간략한 기능 및 사양 텍스트. **숨김(Hide)** 처리.
6. **`Manufacturer` (제조업체) [사내 필수]:**  
   * 부품의 공식 제조사 이름 (예: `Microchip Technology`, `Vishay`, `TI` 등). **숨김(Hide)** 처리.
7. **`PartNumber` (제조사 부품 번호 / MPN) [사내 필수]:**  
   * 주문 및 구매를 위한 공식 제조업체 파트넘버 (예: `MCP2122T-E/SN`). **숨김(Hide)** 처리.
8. **`Cost` (단가) [사내 필수]:**  
   * 부품 단가 기입용 빈 필드 (추후 BOM 연동용). **숨김(Hide)** 처리.


## 4. PCB 풋프린트 및 3D 모델(STP) 연동 규정 (Footprint & 3D Model Rules)

새로운 라이브러리를 추가하거나 요청할 때는 회로도 심볼뿐만 아니라 **PCB 풋프린트(.kicad_mod)**와 **3D 모델(.step/.wrl)** 파일이 대칭 구조로 유기적으로 연동되어야 합니다.

### ① 저장 경로 대칭성 준수 (Symmetric Directory Structure)
모든 카테고리는 심볼, 풋프린트, 3D 모델이 동일한 이름의 폴더(카테고리)에 쌍을 이루어 저장되어야 합니다.
* **심볼:** `symbols/<category>.kicad_sym`
* **풋프린트:** `footprints/<category>.pretty/<part_name>.kicad_mod`
* **3D 모델:** `3dmodels/<category>/<part_name>.step` (또는 `.wrl`)
* *예시 (센서 부품 추가 시):*
  * 심볼: `symbols/common_sensor.kicad_sym` 내에 추가
  * 풋프린트: `footprints/common_sensor.pretty/TFBS4650.kicad_mod` 생성
  * 3D 모델: `3dmodels/common_sensor/TFBS4650.step` 저장

### ② 환경 변수(`${KICAD_GITHUB}`)를 통한 3D 모델 경로 설정
풋프린트 파일 내부에서 3D 모델을 참조할 때 특정 로컬 컴퓨터의 절대 경로를 기록하면 협업 시 경로 깨짐이 발생합니다. 반드시 환경 변수를 사용하여 상대 경로로 연결합니다.
* **❌ 잘못된 예 (절대 경로 사용 금지):**  
  `(model "D:/huni/data/github/Kicad/3dmodels/common_sensor/TFBS4650.step" ...)`
* **⭕ 올바른 예 (환경 변수 치환):**  
  `(model "${KICAD_GITHUB}/3dmodels/common_sensor/TFBS4650.step" (at (xyz 0 0 0)) ...)`

### ③ 라이브러리 요청 시 필수 제출 파일
신규 부품 라이브러리 생성을 요청할 때에는 아래 **3종 파일**을 반드시 패키지로 준비하여 저장소의 지정된 카테고리 경로에 배치해야 합니다:
1. **심볼 사양 및 S-expression 정의**
2. **PCB 풋프린트 파일 (`.kicad_mod` 포맷)**
3. **3D 모델 파일 (`.step` 또는 `.wrl` 포맷)**

### ④ 3종 파일 및 데이터시트 도면 교차 검증 절차 (Reviewer Cross-Check)
라이브러리 및 풋프린트 생성 시, 추정치에 의존하지 않고 제조사 **공식 데이터시트 도면(Dimensions & Recommended Layout)**과 **3종 세트 파일** 간의 핀 번호/패드 피치/치수 정합성을 반드시 교차 검증(Cross-Verification)해야 합니다.
* **검토자(Reviewer) 필수 점검 체크리스트:**
  * [ ] 회로도 심볼 핀 번호/핀 이름과 풋프린트 `.kicad_mod` 패드 번호 간의 1:1 매핑 일치 여부 (데이터시트 핀맵 임의 추정 금지)
  * [ ] 풋프린트 추천 레이아웃 패드 크기(Width/Height) 및 피치(Pitch), Y/X축 좌표계 정합성
  * [ ] 3D STEP CAD 바운딩 박스/좌표계 정량 파싱 분석(`scripts/analyze_step_cad.py`)을 통한 패드 상 물리적 안착(Seating), 1번 핀(Pin 1 Dot) 방향 및 회전 각도(Rotation), PCB 패드 표면(Z=0) 들뜸/파묻힘 없는 XYZ 오프셋(Offset) 완전 밀착 정합성 검증 (정식 CAD STEP만 사용)


  * [ ] 풋프린트 내 3D 모델 상대 경로(`${KICAD_GITHUB}`) 연결 및 3종 파일 정상 등록 여부
  * [ ] 파이썬 검증 스크립트(`core/validator.py`) 실행을 통한 100% PASS 검증 수락 여부




## 5. Git 버전 관리 및 협업 규칙

* **자동 생성 백업 파일 (.bak) 추적 제외:**
  * KiCad 저장 시 자동으로 생성되는 `.bak` 및 `.kicad_sym.bak` 파일은 로컬 설계자의 PC에만 임시 복구용으로 남겨두고, Git 변경 이력의 충돌 방지를 위해 `.gitignore`에 등록하여 원격 저장소 업로드를 차단합니다.
  * 모든 팀원은 리포지토리에 커밋하기 전 불필요하게 추적 중인 백업 파일이 존재하지 않는지 점검합니다.
