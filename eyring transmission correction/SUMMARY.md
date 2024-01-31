# transmission corrections



## 프로젝트 설명
카이랄 분자의 반감기는 그 자체로도 중요하지만, 약학 분야에서 특히 중요한 이슈이다. 약학에서 카이랄성의 중요함을 인식하게 만든 Thalidomide의 경우 point chirality를 가지는 분자인데, 이러한 point chiral 분자는 대게 R/S configuration의 변환이 잘 일어나지 않는다. CH4 같은 분자를 예로 $T_d \rightarrow D_{4h} (or C_s) \rightarrow T_d$ 같은 interconversion의 에너지 베리어는 러프하게 ~10^2 kcal/mol 정도로 알려져있다. 이러한 에너지 장벽을 가진 반응은 상온에서 일어나지 않는다고 생각해도 무방하다. 따라서 초기에 하나의 이성질체만을 선택적으로 합성하거나 정제한다면 약으로써 문제가 되지 않을 가능성이 높다.

반면에 axial chirality를 가지는 분자는 상호 변환 에너지 장벽이 충분히 낮다. 즉 상온이나 체온에서 거울상끼리의 변환이 가능할 수도 있다. 분자 내 카이랄 축을 기준으로 회전하여 상호변환하는 isomerism을 atropisomer라고 하는데, 보통 한쪽 거울상 분자만 유효한 약물작용을 하기때문에 이러한 atropisomerism을 가지는 약물 분자의 반감기를 예측하는 것은 중요하다. 한가지 이성질체로 된 약을 섭취해도 체내에서 다른 이성질체로 변환될 수도 있기 때문이다. 가령 아래 분자를 보자.

![image](https://github.com/kangmg/private/assets/59556369/9b54867c-e0ec-4cb1-800e-230a2167e127)

위 분자는 약물 후보 분자 중 하나인데, [Rowan tutorial](https://docs.rowansci.com/tutorials/dihedral_scans_i)에서 가져왔다. 글을 읽어보면 해당 분자는 B3LYP-D3BJ/pcseg-1 레벨의 계산에서 53.39 kcal/mol 정도의 반응 장벽을 가진다고 예측되며, 500K 정도의 온도에서 450년 정도의 반감기를 가진다고 예측하고 있다.

단 위 튜토리얼에선 trasmission correction을 수행하지 않았다. 이번 프로젝트에선 위에 분자를 포함하여 여러 반응 시스템에 대해서 transmission correaction을 수행하는 걸 목표로 한다.



<br/>


## 기본 이론

보통 Gibbs Free Energy($\Delta G ^{\ddagger}$)가 주어졌을 때 해당 반응에 대한 Rate constant, k는 다음과 같이 구할 수 있다. 아레니우스 식과 유사해보이는데, 엄밀하겐 Transision state theroy로부터 유도된 식이다. 

$$
k(T) = \frac{k_b T}{h} e^{-\Delta G^{\ddagger} /RT }
$$

위에 식의 가정은 Reaction barrier를 넘기 위해선 이보다 큰 에너지를 가진다는 것이다. 즉 classical한 관점을 취한다. 따라서 양자 터널링 효과를 무시한다. 만약 터널링 효과를 고려하고 싶다면, 터널링 상수(transmission coefficient), $\kappa$를 도입하면 된다. 따라서 다음과 같이 식을 수정할 수 있다. 아래 식을 eyring equation이라고 부른다.

$$
k(T) = \kappa(T)\frac{k_b T}{h} e^{-\Delta G^{\ddagger} /RT }
$$

이때 적절한 속도상수 k로부터 [ee]의 반감기는 다음과 같이 계산될 수 있다

$$
t_{1/2} =  \frac{ln2}{k_{rac}}
$$

<br/>

## 반응 시스템

1. 위에서 언급한 atropisomer의 interconversion 시스템

2. 

<br/>


## 계산 결과 요약 
1. 

## 계산 디테일 링크
