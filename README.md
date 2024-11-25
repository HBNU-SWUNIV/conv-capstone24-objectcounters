# 한밭대학교 인공지능소프트웨어학과 ObjectCounters팀

**팀 구성**
- 20221047 김윤희 
- 20221052 서민경
- 20231052 김현정

## <u>Teamate</u> Project Background
- ### 필요성
    - 자율 주행 기술은 운전자의 개입을 최소화하거나 완전히 제거하여 최적의 주행 경로를 선택하여 스스로 주행하도록 하여 교통사고 감소, 교통 혼잡 완화 등의 목표를 추구하고 있다. 그 과정에서 안전성과 효율성을 위해 도로 위의 차량, 보행자, 신호 등의 차량 주변 상황을 감지하고 상황에 맞게 차량을 제어해야 한다. 자율 주행 시스템은 정확하고 빠른 의사결정을 통해 차량 주변을 분석하고 대처하는 기술을 필요로 한다.
  - 자율주행 시스템에 다양한 객체 탐지 기술은 점점 더 상용화되고 있고 자율 주행 차량의 법규와 안전 기준이 마련되고 있으며 안정적이고 신뢰할 수 있는 객체 탐지 기술에 대한 요구가 커지고 있다.
  - 상용화된 자율 주행 차량은 고속도로에서의 주행 보조 기능을 통해 객체 탐지 기술을 사용하여 주행 중 다른 차량, 보행자, 장애물을 인식하고 반응하나 현재 기술은 객체를 탐지하는 데 높은 수준에 도달했지만, 객체의 행동을 예측하는 능력은 아직 발전이 필요한 단계이다. 따라서 객체의 행동 중 좌회전, 우회전 등을 방향 지시등을 감지하여 예측을 더욱 정교하게 할 수 있다.
  
- ### 기존 해결책의 문제점
  
  #### 문제점
  - **비용적 문제** : 현재 자율주행은 3D 이미지 기반 분석과 라이다, 레이더 등 고가의 센서를 활용하는데 이는 자율주행 시스템의 비용을 높이는 주요 원인이다. 이에 비해 2D 이미지 기반 기술은 상대적으로 저렴하고,   기존 카메라 장비를 활요할 수 있어 비용 효율적인 대안이 될 수 있다.
  - **안전성 문제** : 차량의 이동 방향과 의도를 정확히 파악하지 못하면 충돌이나 예측 불가능한 사고의 위험이 증가한다.
  - **복잡한 도로 환경 문제** : 교차로나 고속도로처럼 차량, 보행자, 교통 신호 등이 혼재한 상황에서는 특정 차량의 동작을 분리하고 분석하기 어렵다.
    
  #### 해결방안
  - **비용적 문제** : 2D 이미지 기반 기술을 통해 기존의 저비용 카메라 장비를 활용하여 비용을 절감하면서도 성능을 확보할 수 있는 방안을 제안하여 경제성을 갖춘다.
  - **안전성 문제** : 딥러닝 기반 객체탐지과 객체 추적 기술을 결합하여 차량의 이동 방향과 의도를 정확히 파악함으로써 충돌 위험을 감소시킨다.
  - **복잡한 도로 환경 문제** : 옵티컬 플로우를 통해 각 차량의 세부적인 움직임을 추적하고, 이를 방향지시등 정보와 통합함으로써 차량의 의도와 경로를 보다 정밀하게 분석할 수 있다. 
  
## System Design
---------------------------------------------
  - ### System Requirements
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
      
    <img width="539" alt="image" src="https://github.com/user-attachments/assets/5b434e4d-c8ac-4e92-897c-f9f0a0599960">


      계층적 디텍션을 통해 YOLO가 방향지시등을 0개, 1개, 2개로 디텍션할 경우로 Case를 나누어서 각자 다른 Task를 수행하도록 한다.
      1. 계층적 디테션
      2. 방향지시등 ON-OFF 알고리즘
      3. 차량 이동 방향 예측(옵티컬 플로우)
         
    - Design
   
      ![image](https://github.com/user-attachments/assets/063dccd6-bae2-4158-add4-0ef60259a181)
      ![image](https://github.com/user-attachments/assets/f10e1d18-468b-499e-8b01-07477200c825)

      
      - 방향지시등이 켜졌을 때 : "(차 번호)의 (Car-L or Car-R)의 방향지시등이 켜졌습니다. 속도를 줄이세요"
      - 좌회전, 우회전 시 : ex) 차량이 우회전하고 있습니다. 속도를 줄이세요.
      - defult : "속도를 유지하세요."
 ----------------------------------------------
    
## Case Study
  - ### Description
    - 프로젝트 개요
      - 차량의 이동 방향을 예측하고 분석하는 주행 보조 시스템은 운전자가와 주변 차량 간의 효과적인 소통을 위해 필요함.
      - 3D 이미지의 한계인 계산 복잡도와 비용을 최소화하기 위해 2D 이미지 사용
      - 계층적 객체 탐지 알고리즘을 통해 객체를 탐지할 뿐만 아니라 차량과 방향 지시등을 하나의 그룹으로 묶고 객체 추적을 통해 차량의 방향지시등 점등을 감지하고 방향지시등 객체를 탐지하지 못했거나 반대편 차량을 옵티컬 플로우를 계산하여 복잡한 환경에서도 수행가능하도록 설계함.

---------------------------------------------------
## Conclusion

- 알고리즘 수행 결과
  
  - 계층적 디텍션 : 
    
    ![image](https://github.com/user-attachments/assets/01db8e69-9b0d-48e7-a960-54c3cff73075)


  - 방향지시등 ON-OFF 알고리즘 :
    ![image](https://github.com/user-attachments/assets/43e31e6b-3b39-4ba2-814f-8176cadb509f)

  - 차량 이동 방향 예측(옵티컬 플로우) : 
    ![image](https://github.com/user-attachments/assets/44525876-880d-4278-93c7-5ddee0b745c0)

- 전체결과
![image](https://github.com/user-attachments/assets/9a64441f-7bdb-407e-a7be-b18da02e4d51)

  이 연구에서는 차량 방향 예측과 방향지시등 점등 감지 기능을 통합한 주행 보조 시스템을 설계하고 구현하였다. 본 시스템은 계층적 객체탐지를 통해 차량 방향 예측과 방향지시등 점등 여부를 분석하여 다양항 주행 환경과 조건에서도 운전자가 안정적이게 주행할 수 있도록 속도 제어 메세지를 출력한다. 

-------------------------------------------------

## Project Outcome
- ### 2025년 한국로봇공학회 

