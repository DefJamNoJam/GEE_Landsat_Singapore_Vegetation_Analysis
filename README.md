# 🌳 싱가포르 도시정원 수립계획 분석 : Landsat 위성영상을 활용한 변화 관찰

## 🚀 프로젝트 개요

본 프로젝트는 Landsat 위성영상을 활용하여 싱가포르의 도시정원 수립계획(City in a Garden), 특히 Park Connector 계획이 싱가포르의 녹지 공간에 미친 영향을 관찰하고 분석합니다. 싱가포르가 '도심 속 녹지'를 구현하기 위해 노력한 결과와, 그로 인해 발생한 지형 및 식생 변화를 시계열적으로 비교 분석하는 것을 목표로 합니다.

## 🛠️ 사용 기술 및 데이터

* **위성 데이터:**
    * **과거 싱가포르 (1985년 1월 1일 ~ 1990년 5월 30일):** Landsat 5 위성의 LANDSAT/LT05/C02/T1 이미지 컬렉션을 활용했습니다. 이는 Park Connector 계획이 수립된 1991년 이전의 데이터를 나타냅니다.
    * **현재 싱가포르 (2022년 1월 1일 ~ 2024년 5월 30일):** Landsat 9 위성의 LANDSAT/LC09/C02/T1 이미지 컬렉션을 활용했습니다.
* **플랫폼:** Google Earth Engine (GEE).
* **라이브러리:** Python `geemap` 라이브러리.

## 🔍 분석 방법

`SINGAPORE.ipynb` Jupyter Notebook 파일에는 다음 분석 단계가 포함되어 있습니다.

1.  **Raw Image 비교:**
    * 과거(1985-1990) Landsat 영상은 밴드 **B3, B2, B1**을 사용하여 시각화했습니다.
    * 현재(2022-2024) Landsat 영상은 밴드 **B4, B3, B2**를 사용하여 시각화했습니다.
    * `geemap.split_map` 기능을 활용하여 과거와 현재의 영상을 나란히 비교하여 변화를 직관적으로 파악했습니다.

2.  **식생지수(NDVI) 비교:**
    * **NDVI(Normalized Difference Vegetation Index)**는 식생의 밀도와 활성도를 나타내는 지수로, 과거(1985-1990)와 현재(2022-2024) 싱가포르의 식생 변화를 정량적으로 분석하는 데 사용되었습니다.
    * 현재 NDVI(`ndvi_present`)는 `landsat_2022_2024` 영상의 **B5 밴드(NIR)와 B4 밴드(Red)**를 이용하여 `normalizedDifference` 함수로 계산되었습니다.
    * `geemap.split_map`을 통해 과거와 현재의 NDVI 맵을 비교했으며, 특정 임계값(threshold)을 기준으로 식생 지역(`vegetation_past`, `vegetation_present`)을 추출하여 시각화했습니다.

3.  **녹지 면적 변화 분석:**
    * 증가한 녹지(`increased_vegetation`) 및 감소한 녹지(`decreased_vegetation`) 지역을 식생지수 변화를 기반으로 식별했습니다.
    * `geemap.zonal_stats` 함수를 사용하여 싱가포르 전체(`country` 지오메트리) 내에서 증가 및 감소한 녹지의 면적을 계산했습니다.
        * **줄어든 녹지 면적:** 34,510.5882 m²로 계산되었습니다. 이는 99.6 – 61.254902 = 38.345098이라는 값에 Landsat 위성 픽셀값(30x30m)을 곱하여 산출된 결과입니다.
        * **늘어난 녹지 면적:** 7,140 m²로 계산되었습니다.
        * **참고:** 원본 자료에서 면적 계산 과정이 상세히 제시되어 있으며, 최종적으로 늘어난 녹지보다 줄어든 녹지 면적이 더 크게 나타났습니다.

4.  **Timelapse 시각화:**
    * 1985년부터 2024년까지의 Landsat 위성영상을 활용하여 싱가포르의 지형 변화를 보여주는 타임랩스 GIF/MP4를 생성했습니다.
    * `geemap.landsat_timelapse` 함수를 사용하여 `singapore_landsat_timelapse_19852024.gif` 파일을 생성하였고, 이는 싱가포르 녹지 변화의 동적인 모습을 시각적으로 제공합니다.
    * **타임랩스 GIF 파일은 `timelapse/singapore_landsat_timelapse_19852024.gif` 경로에서 확인하실 수 있습니다.**

## 📊 분석 결과 및 결론

* **녹지 면적의 변화:** Park Connector 계획의 결과로 전체적인 녹지 면적은 늘어났지만, 분석 결과에 따르면 예상보다 줄어든 부분도 상당수 관찰되었습니다. 이는 늘어난 녹지 면적(7,140m²)보다 줄어든 녹지 면적(34,510.5882m²)이 더 크다는 면적 비교 결과에서도 나타납니다.
* **도심지 녹지 재배치:** 위성영상을 통해 관찰했을 때, 전체적으로 도심지 주변의 녹지가 더욱 정돈되고 재배치된 느낌을 줍니다. 이는 단순히 면적 확장보다는 효율적인 공간 활용에 중점을 둔 것으로 해석될 수 있습니다.
* **Park Connector 계획의 진정한 목적:** Park Connector는 단순히 녹지 면적을 늘리는 것이 목적이 아니며, 주요 공원과 녹지, 공간 등을 연결하여 다목적으로 활용하는 종합적인 계획입니다.
* **현재 진행형:** Park Connector 계획은 현재 진행 중인 프로젝트이며, 2030년까지 계획되어 있어 앞으로 더 많은 녹지 확충과 더욱 뚜렷한 변화를 위성영상으로 관찰할 수 있을 것으로 예상됩니다.

## 💡 위성영상으로 관찰하였을 때 기대할 수 있는 변화

* **과거보다 확장된 녹지:** 싱가포르 전역에서 녹지 면적이 더욱 확장될 것으로 기대됩니다.
* **식물로 뒤덮인 건물:** 건물들이 식물로 뒤덮여 '도심 속 녹지'의 모습을 강화할 것입니다.
* **Green Corridor:** 녹지와 녹지 사이를 유기적으로 연결하는 '그린 코리더(Green Corridor)'가 형성되어 도시 전체의 생태 연결성을 높일 것입니다.

## ⚙️ 기술 스택

* **언어:** Python
* **플랫폼:** Google Earth Engine (GEE)
* **라이브러리:** `geemap`, `earthengine-api`, `numpy`, `pandas`, `matplotlib`, `folium` (Jupyter Notebook에서 사용될 가능성)
* **데이터:** Landsat 5, Landsat 9 위성 이미지 컬렉션

## 🚀 프로젝트 실행 방법

1.  **저장소 클론:**
    ```bash
    git clone [https://github.com/YourUsername/GEE_Landsat_Singapore_Vegetation_Analysis.git](https://github.com/YourUsername/GEE_Landsat_Singapore_Vegetation_Analysis.git)
    cd GEE_Landsat_Singapore_Vegetation_Analysis
    ```
    (여기서 `YourUsername`은 본인의 GitHub 사용자 이름으로 대체하세요.)

2.  **Google Earth Engine(GEE) 인증:**
    * GEE Python API를 사용하려면 [Google Earth Engine](https://earthengine.google.com/) 계정이 필요하며, 로컬 환경에서 GEE Python API 인증을 완료해야 합니다.

3.  **가상 환경 설정 및 라이브러리 설치:**
    ```bash
    python -m venv venv
    source venv/bin/activate  # Windows: .\venv\Scripts\activate
    pip install -r requirements.txt
    ```
    * `requirements.txt` 파일은 프로젝트 실행에 필요한 모든 Python 라이브러리(예: `geemap`, `earthengine-api`, `numpy`, `pandas`, `matplotlib`, `folium` 등)를 포함해야 합니다.

4.  **Jupyter Notebook 실행:**
    ```bash
    jupyter notebook Notebooks/SINGAPORE.ipynb
    ```
    * 웹 브라우저에서 Jupyter Notebook이 열리면, 각 셀을 순서대로 실행하여 분석 과정을 따라갈 수 있습니다.

## 📚 참고 자료

* [프로젝트 발표 자료 (PDF)](./docs/singapore_landsat.pdf) 
