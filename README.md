# 한밭대학교 인공지능소프트웨어학과 ObjectCounters팀

**팀 구성**
- 20221047 김윤희 
- 20221052 서민경
- 20231052 김현정

## <u>Teamate</u> Project Background
- ### 필요성
  자율 주행 기술은 운전자의 개입을 최소화하거나 완전히 제거하여 최적의 주행 경로를 선택하고 스스로 주행하도록 하여 교통사고 감소, 교통 혼잡 완화 등의 목표를 추구하고 있다. 그 과정에서 안전성과 효율성을 위해 도로 위의 차량, 보행자, 신호 등의 차량 주변 상황을 실시간으로 감지하고 상황에 맞게 차량을 제어해야 한다. 자율 주행 시스템은 정확하고 빠른 의사결정을 통해 차량 주변을 분석하고 대처하는 기술이 필요하다. 이 과제의 주된 목적은 객체 탐지 기술을 활용하여 차량의 방향 지시등을 탐지하여 자율 주행 차량의 주행 보조 시스템을 개발하는 것이다. 이 기술을 통해 다른 차량의 방향을 예측할 수 있다면 자율 주행 시스템은 도로와 교차로 등의 상황에서 주행의 안전성을 향상할 수 있다. 현재 테슬라, 구글 등 여러 기업이 자율주행 시스템에 다양한 객체 탐지 기술을 적용하고 있으며 점점 더 상용화되고 있고 자율 주행 차량의 법규와 안전 기준이 마련되고 있으며 안정적이고 신뢰할 수 있는 객체 탐지 기술에 대한 요구가 커지고 있다. 상용화된 자율 주행 차량은 고속도로에서의 주행 보조 기능을 통해 객체 탐지 기술을 사용하여 주행 중 다른 차량, 보행자, 장애물을 인식하고 반응하나 현재 기술은 객체를 탐지하는 데 높은 수준에 도달했지만, **객체의 행동을 예측하는 능력은 아직 발전이 필요한 단계**이다. 따라서 객체의 행동 중 좌회전, 우회전 등을 방향 지시등을 감지하여 예측을 더욱 정교하게 할 수 있다.
  
- ### 기존 해결책의 문제점
  #### 문제점
  
  - **비용적 문제** : 현재 자율주행은 3D 이미지 기반 분석과 라이다, 레이더 등 고가의 센서를 활용하는데 이는 자율주행 시스템의 비용을 높이는 주요 원인이다. 이에 비해 2D 이미지 기반 기술은 상대적으로 저렴하고,   기존 카메라 장비를 활요할 수 있어 비용 효율적인 대안이 될 수 있다.
  - **안전성 문제** : 차량의 이동 방향과 의도를 정확히 파악하지 못하면 충돌이나 예측 불가능한 사고의 위험이 증가한다.
  - **복잡한 도로 환경 문제** : 교차로나 고속도로처럼 차량, 보행자, 교통 신호 등이 혼재한 상황에서는 특정 차량의 동작을 분리하고 분석하기 어렵다.
  #### 해결방안

  - **비용적 문제** : 2D 이미지 사용
  - **안전성 문제** : 차량의 이동 방향과 의도를 정확히 파악하지 못하면 충돌이나 예측 불가능한 사고의 위험이 증가한다.
  - **복잡한 도로 환경 문제** : 교차로나 고속도로처럼 차량, 보행자, 교통 신호 등이 혼재한 상황에서는 특정 차량의 동작을 분리하고 분석하기 어렵다.
  
## System Design
  - ### System Requirements
    
    Base ----------------------------------------
    - pip==24.3.1
    - python==3.8.20
    - numpy==1.23.1
    - ultralytics==8.3.27
    - opencv-python==4.6.0.66
    - opencv-python-headless==4.10.0.84 
    - opencv-contrib-python==4.6.0.66
    - scipy==1.10.0
    - torch==2.4.1
    - torchvision==0.19.1
    - deep-sort-realtime==1.3.2
    ----------------------------------------------

  - ### System Design

    - 시스템 동작 순서도
      ![image](https://github.com/user-attachments/assets/17d0553f-0042-43e8-8518-0de9922e8559)

    - Design
      ![image](https://github.com/user-attachments/assets/063dccd6-bae2-4158-add4-0ef60259a181)
      ![image](https://github.com/user-attachments/assets/b55d8546-53b3-4a5c-9eb7-64fb9bddea35)
      - 방향지시등이 켜졌을 때 :
      - 좌회전, 우회전 시 :
      - defult : "속도를 유지하세요."

    
## Case Study
  - ### Description

    본 연구는 차량 방향 예측 및 방향지시등 점등 감지 주행 보조 시스템 설계 및 구현을 중심으로 진행되었다. 이 시스템은 운전자의 방향지시등 점등을 분석하고 차선 변경 및 회전과 같은 차량의 움직임을 예측함으로써,  운전자가 놓칠 수 있는 상황에서도 차량의 의도를 정확하게 파악하는 운전자의 인지 및 반응 능력을 보완한다.이를 위해 YOLO 기반 객체 탐지와 계층적 객체 탐지, Deesort 기반 트래킹, 그리고 RGB 분석을 활용한 방향지시등 점등 감지 알고리즘을 적용하였다. 이 연구에서는 시스템 개발 과정에서 직면했던 주요 도전 과제, 다양한 조건에서의 결과 정확도 등의 어려움 등을 다루며 이를 위해 어떤 접근법이 사용되었는지를 설명한다. 나아가 이 시스템이 실제 주행 영상에서 테스트 되었을 때, 얻은 결과를 토대로 더욱 안전한 자율 주행 시스템의 개발에 기여할 수 있는지를 탐구한다.
  
## Conclusion
  - 계층적 디텍션 :
    
![image](https://github.com/user-attachments/assets/6a3b97b1-b5d5-4c5c-b551-d214ef340eff)

  - 방향지시등 ON-OFF 알고리즘 :
![image](https://github.com/user-attachments/assets/43e31e6b-3b39-4ba2-814f-8176cadb509f)

  - 차량 이동 방향 예측(옵티컬 플로우) : 
![image](https://github.com/user-attachments/assets/44525876-880d-4278-93c7-5ddee0b745c0)

- 전체결과

  이 연구에서는 차량 방향 예측과 방향지시등 점등 감지 기능을 통합한 주행 보조 시스템을 설계하고 구현하였습니다. 본 시스템은 계층적 객체탐지를 통해 차량 방향 예측과 방향지시등 점등 여부를 분석하여 다양항 주행 환경과 조건에서도 안정적이게 

  
## Project Outcome
- ### 2025년 한국로봇공학회 

