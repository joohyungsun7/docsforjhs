Paper 
=====

.. _Introduction:

논문 스터디. M. Dayarathna, Y. Wen and R. Fan, "Data Center Energy Consumption Modeling: A Survey," in IEEE Communications Surveys & Tutorials, vol. 18, no. 1, pp. 732-794, Firstquarter 2016, doi: 10.1109/COMST.2015.2481183.

.. note::

  Open Access

org
----------------

IV. DIGITAL CIRCUIT LEVEL ENERGY CONSUMPTION MODELING

Power models play a fundamental role in energy-efficiency
research of which the goal is to improve the components’
and systems’ design or to efficiently use the existing hardware
[7]. Desirable properties of a full system energy consumption
model include Accuracy (accurate enough to allow the desired
energy saving), Speed (generate predictions quickly enough),
Generality and portability (should be suitable for as many
systems as possible), Inexpensiveness (should not requires expensive
or intrusive infrastructure), and Simplicity [73]. This
section provides an introduction to the energy consumption
fundamentals in the context of electronic components.

A. Energy vs Power
Energy (E) is the total amount of work performed by a
system over a time period (T) while power (P) is the rate at
which the work is performed by the system. The relationship
between these three quantities can be expressed as,
E = PT, (3)
where E is the system’s energy consumption measured in
Joules, P is measured in Watts and T is a period of time
measured in seconds. If T is measured in unit times then the
values of energy and power become equal.
The above expression can be slightly enhanced by considering
energy as the integration of power values in a time period
starting from t1 and ends at t2. Note that we use the terms energy
and power interchangeably in this paper.
B. Dynamic vs Static Power
Complementary metal-oxide semiconductor (CMOS) technology
has been a driving force in recent development of
computer systems. CMOS has been popular among the microprocessor
designers due to its resilience for noise as well
as low heat produced during its operation compared to other
semiconductor technologies. The digital CMOS circuit power
consumption (Ptotal) can be divided into two main parts as,
Ptotal = Pdynamic + Pstatic, (4)
where Pdynamic is the dynamic power dissipation while Pstatic is
the static power dissipation.
Dynamic power is traditionally thought of as the primary
source of power dissipation in CMOS circuits [74]. The three
main sources of dynamic power (Pdynamic) consumption in
digital CMOS circuits are switched capacitance power (caused
by the charging and discharging of the capacitive load on each
gate’s output), short-circuit power (caused by short-circuit current
momentarily flowing within the cell), and leakage power
(caused by leakage current irrespective of the gate’s state) [75].
These power components can be represented as follows,
Pdynamic = Pswitching + Pshort−circuit + Pleakage, (5)
where the first term Pswitching represents the switching component’s
(switching capacitance) power. The most significant
component of power discharge in a well designed digital circuit
is the switching component. The second term represents the
power consumption that happens due to the direct-path short
circuit current that occurs when both the N-type metal-oxide
semiconductor (NMOS) and P-type metal-oxide semiconductor
(PMOS) transistors are simultaneously active making the current
directly flow to ground. The leakage current creates the
third component which is primarily determined by the fabrication
technology of the chip. In some types of logic styles (such
as pseudo-NMOS) a fourth type of power called static biasing
power is consumed [76]. The leakage power consists of both
gate and sub-threshold leakages which can be expressed as [77],

Pleakage = ngateIleakageVdd,
Ileakage = AT2e−B/T + Ce(r1Vdd+r2)
, (6)
where ngate represents the transistor count in a circuit while
Ileakage represents the leakage current, and T corresponds to the
temperature. The values A, B, C, r1, and r2 are constants. Circuit
activities such as transistor switches, changes of values in registers,
etc. contribute to the dynamic energy consumption [78].
The primary source of the dynamic power consumption is
the switched capacitance (Capacitive power [79]). If we denote
A as the switching activity (i.e., Number of switches per clock
cycle), C as the physical capacitance, V as the supply voltage,
and f as the clock frequency; the dynamic power consumption
can be defined as in Equation (7) [80]–[82],
Pcapacitive = ACV2f . (7)
Multiple techniques are available for easy scaling of the supply
voltage and frequency in large range. Therefore, the two parameters
V and f attract a large attention by the power-conscious
computing research.
Static power (Pstatic) is also becoming and important issue
because the leakage current flows even when a transistor
is switched off [78] and the number of transistors used in
processors is increasing rapidly. Static power consumption of
a transistor can be denoted as in Equation (8). Static power
(Pstatic) is proportional to number of devices,
Pstatic ∝ IstaticV, (8)
where Istatic is the leakage current.
The above mentioned power models can be used to accurately
model the energy consumption at the micro architecture
level of the digital circuits. The validity of such models is a
question at the higher levels of the system abstractions. However,
as mentioned in Section I such basic power models have
been proven to be useful in developing energy saving technologies.
For example the power model described in Equation (7)
makes the basis of dynamic voltage frequency scaling (DVFS)
technique which is a state-of-the-art energy saving technique
used in current computer systems.


V. AGGREGATE VIEW OF SERVER ENERGY MODELS
IT systems located in a data center are organized as components.
Development of component level energy consumption
models helps for multiple different activities such as new
equipment procurement, system capacity planning, etc. While
some of the discussed components may appear at different
other levels of the data center hierarchy, all of the components
described in this section are specifically attributed to servers.
In this section we categorize the power models which provide
aggregated view of the server power models as additive models,
utilization based models, and queuing models.
A. Additive Server Power Models
Servers are the source of productive output of a data center
system. Servers conduct most of the work in a data center and
they correspond to considerable load demand irrespective of
the amount of space they occupy [83]. Furthermore, they are
the most power proportional components available in a data
center which supports implementation of various power saving
techniques on servers. In this sub section we investigate on
the additive power models which represent the entire server’s
power consumption as a summation of its sub components. We
follow an incremental approach in presenting these power models
starting from the least descriptive models to most descriptive
models. These models cloud be considered as an improvement
over linear regression, where non-parametric functions are used
to fit model locally and are combined together to create the
intended power model [84].
One of the simplest power models was described by
Roy et al. which represented the server power as a summation
of CPU and memory power consumption [85]. We represent
their power model as,
E(A) = Ecpu(A) + Ememory(A), (9)
where Ecpu(A) and Ememory(A) are energy consumption of the
CPU and the memory while running the algorithm A. More
details of these two terms are available in Equations (54) and
(87) respectively. Jain et al. have described a slightly different
power model to this by dividing the energy consumption of

trs
----------------

IV. 디지털 회로 수준의 에너지 소비 모델링

전력 모델은 에너지 효율성 연구에서 기본적인 역할을 수행하며, 이 연구의 목표는 구성 요소 및 시스템의 설계를 개선하거나 기존 하드웨어를 효율적으로 사용하는 것입니다 [7]. 전체 시스템의 에너지 소비 모델의 원하는 특성에는 정확성 (원하는 에너지 절약을 가능하게 하는 충분히 정확함), 속도 (예측을 충분히 빠르게 생성함), 일반성 및 이식성 (가능한 많은 시스템에 적합함), 비용 효율성 (비싼 또는 침입적인 인프라가 필요하지 않음) 및 간단함 [73]이 포함됩니다. 이 섹션은 전자 구성 요소의 에너지 소비 기본 원리에 대한 소개를 제공합니다.
