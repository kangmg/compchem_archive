# [Butadiene의 dihedral rotational barrier를 EDA로 분석해보기]

![butadiene](https://github.com/kangmg/compchem_archive/assets/59556369/5f63a655-c6cf-45e7-9955-836c708bd7df)

<br/>

### 프로젝트 설명
Butadiene의 rotational barrier를 보면, cis-butadiene에서 local minima가 생기는 것이 아니라 살짝 더 비틀린 형태에서 local minima가 생긴다. EDA 계산을 통해 이 구조적 비틀림의 원인을 알아보고자 하였다. <a href="https://link.springer.com/article/10.1007/s11224-014-0557-5">Ethane의 rotational barrier에 관한 연구</a>에서 알 수 있듯이 frozen scan과 relaxed scan에 따라서 결과가 달라질 수 있기에 두 반응좌표 모두에 대해서 계산을 수행해보았다. 이후 relaxed scan의 EDA에선 local minima를 확인하지 못하였고, distortion에 의한 영향을 확인하기 위해서 추가적으로 Activation strain analysis를 진행하였다.


|계산_종류|프로그램|
|:-:|:-:|
|GKS_EDA|XEDA 1.0 (GAMESS-US)|
|sobEDA|gaussian/sobEDA code|
|Coordinate scan|PSI4|
|인풋 생성 & 데이터 처리|EDA-support (beta)|
|ASA 계산| <a href="https://github.com/dsvatunek/autoDIAS">autoDIAS 코드</a>, PSI4|

<br/>

### 결과 정리
우선 PSI4로 부터 구한 coordinate scan결과로부터 에너지를 이면각에 대해서 플롯하였다. 기본적으로 frozen scan의 경우 constraint를 더 주는 PES scan이므로 전반적으로 에너지가 높게 계산된다. 에너지는 global minima에 대한 relative energy이다.

![frozenvsrelaxed](https://github.com/kangmg/compchem_archive/assets/59556369/17d7d909-afb2-4df9-8b7d-8dfd77443edc)

아래는 해당 좌표들에 대해서 EDA 계산을 수행한 결과이다.


![relaxed_scan2](https://github.com/kangmg/compchem_archive/assets/59556369/c8453827-4021-4bcb-a72f-27f4ea05ba55)

![frozen_scan2](https://github.com/kangmg/compchem_archive/assets/59556369/70ced002-4c91-47df-95c2-f4c0a4c6c7f1)

두 좌표계에 대한 결과가 어느정도 차이가 있을거라곤 어느정도 예상했지만 전혀 다른 결과를 보여주었다. 결과를 해석해보면 다음과 같다.


#### frozen dihedral scan 결과

* Frozen scan의 경우 $E_{int}$ 자체가 rotational PES가 된다.
* 이 그래프로부터 알 수 있는 점은 pauli repulsion에서 비슷한 위치에 minima가 생기므로 평면에서 약간 비틀린 구조일 때 filled orbital의 overlap이 가장 작을 것이라고 예상할 수 있다.
* 히물며 pauli repulsion에서 기인하여 local minima가 생긴다고 이해할 수 있다.


#### relaxed dihedral scan 결과
* 반면에 Relaxed scan의 경우는 좀 다르다. 우선 $E_{int}$ 자체가 rotational PES는 아니다. distortion을 고려해야 rotational PES가 된다. 이에 대해선 이후 글에서 다뤄보겠다.
* 흥미로운 점은 relaxed scan의 경우 검은 색 선을 보면 local minima를 보이지 않는다는 것이다. (참고로 EDA 계산과 이후 ASA 계산에서 fragmentation은 C1-C2-C3-C4 중 C2-C3 결합에 대해서 나눴다.)
* 이 결과로부터 예상할 수 있는 점은 $CH_2CH$의 relaxation이 이 local minma를 만드는데 중요한 역할을 한다는 것이다.
* 이 결과는 fragmentation을 어떻게 하느냐에 따라 EDA가 주는 통칠이 크게 달라질 수 있음을 의미하기도 하는데 역시 이후 글에서 다뤄보겠다.

Relaxed scan의 ASA 계산 결과이다. Fragmentation은 동일하게 C2-C3에 대해서 나눴다. 결과적으로 각각의 fragments가 회전 중에 relaxation하며 생기는 에너지 감소(= $E_{distortion}$ )로 인해 local minima가 생겼다. 

![asm](https://github.com/kangmg/compchem_archive/assets/59556369/2307e738-b38b-47fc-a731-934d61c5fc66)

$E_{int}$ 자체에 local minima가 없기에 EDA 계산이 local minima에 대해서 주는 정보는 그다지 없다. 따라서 이후에 다른 fragmentation에 대해서도 EDA를 진행해볼 예정이다.

참고로 위에 ASA 계산에서 구한 $E_{int}$는 EDA와 동일한 b3lyp/aug-cc-ptvz 로 구했지만 전혀 다른 알고리즘으로 구해졌다.

* $E_{int}(EDA) = E_{ES} + E_{POL} + E_{CORR} + E_{EX} + E_{REP} $

* $E_{int}(ASA) = E_{tot} - E_{dis} $


그럼에도 거의 동등한 값을 보여준다.

![int_rel_comparison](https://github.com/kangmg/compchem_archive/assets/59556369/df3dd855-157c-481c-8e52-e5da2620b738)


분해된 EDA 텀들을 제외하고 모두 plot한 그래프다.

![all](https://github.com/kangmg/compchem_archive/assets/59556369/345bb482-ea0c-412c-b16c-6b0df3e292bf)

위에 계산들은 XEDA로 한 계산들이고 sobEDA로도 frozen dihedral rotation에 대한 계산을 동등하게 수행해보았다. 역시 같은 결과를 얻을 수 있다.

![Figure](https://github.com/kangmg/compchem_archive/assets/59556369/6be794e8-f9bb-4b80-bf2e-f36a69f07182)

<br/>

### 계산 디테일
Rotational barrier와 reaction coordinates는 PSI4 프로그램을 이용하여 계산하였으며 ccsd/6-31g로 수행하였다. 이렇게 얻은 반응좌표에 대해서 EDA 계산을 수행하였다. EDA 방법론은 GKS-EDA 방법론을 사용하였으며 b3lys/aug-cc-pvtz 레벨에서 계산되었다. 프로그램은 XEDA 1.0을 GAMESS-US 2021-R2 버전에 패치시킨 후 사용하였다. XEDA 1.0 프로그램의 인풋 생성과 데이터 처리는 현재 개발중인 EDA-support 코드를 사용하였다. ASA 계산은 <a href="https://github.com/dsvatunek/autoDIAS">autoDIAS 코드</a>를 이용하였으며, PSI4 계산을 사용할 수 있도록 코드를 수정 후 b3lyp/aug-cc-pvtz 레벨에서 계산되었다.

<br/>

### 보충 파일 [goto SI](https://github.com/kangmg/compchem_archive/tree/main/butadiene%20rotational%20barrier/SI)

|파일 이름 | 파일 설명 | 
|:---:|:---:|
|SI/outputs/*|XEDA 1.0의 output 파일|
|SI/inputs/*|XEDA 1.0의 input 파일|
|SI/coordinates/relaxed_scan.xyz|relaxed scan에 대한 rotational coordiante|
|SI/coordinates/frozen_scan.xyz|frozen scan에 대한 rotational coordiante|
|SI/images/*.png|계산 결과 파일|
|SI/relaxed_scan_ASA.inp|ASA input 파일|
|SI/relaxed_scan_ASA.txt|ASA output 파일|
|SI/sobEDA_output.zip|sobEDA input 및 output 파일|

<br/>

### Contact
contact me via kangmg@korea.ac.kr
