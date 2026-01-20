---
icon: computer
---

# Simplified Oven Thermal Simulation

Software: Autodesk Fusion&#x20;

Official tutorial: [https://help.autodesk.com/view/fusion360/ENU/?guid=SIM-TA](https://help.autodesk.com/view/fusion360/ENU/?guid=SIM-TA)

Platform: Maccbook (M1)

<figure><img src="../.gitbook/assets/copper_peek.png" alt="" width="375"><figcaption></figcaption></figure>

## Method

### Choice of Simulation Method

The objective of this simulation is to determine the steady-state temperature distribution inside an oven subjected to a heater and heat losses through its surfaces. At steady state, the system reaches thermal equilibrium. Two of the most relevant numerical simulation methods for this application are discussed below.

**Finite Element Method (FEM)**&#x20;

The finite-element method is a computational method that subdivides a CAD model into very small but finite-sized elements of geometrically simple shapes. The collection of all these simple shapes constitutes the so-called finite-element mesh.

It naturally handles complex 3D geometries, multiple materials, and mixed boundary conditions, well suited for solid heat conduction problems, especially efficient for steady-state solutions. \[ref7]

Another simulation method which might be suitable for our case is:&#x20;

**Finite Volume Method (FVM)**&#x20;

The finite-volume method is similar to the finite-element method in that the CAD model is first divided into very small but finite-sized elements of geometrically simple shapes. It is based on the fact that many physical laws are conservation laws—what goes into one cell on one side needs to leave the same cell on another side. It enforces energy conservation over small control volumes, widely used in fluid flow and CFD (computational fluid dynamics).

Thus, Finite Element Method would be a better choice for our case, and Fusion simulation is exactly a tool capable of doing thermal simulation based on FEM method. \[ref8]

### Add Heating Effect

There are three load types which can be used as heat source:

* Applied temperature
* Heat source
* Internal heat

In our setup, the temperature sensor is placed inside a pinhole at lower part of oven with a stable temperature controlled by PID temperature controller. As an approximation, choose applied temperature and apply it on a square region at the oven bottom corresponding to the contact area with the heater. For the region on the PEEK base that is in contact with the heater bottom, a heating effect could also be applied; however, this is not the focus of the present study. Here, the same thermal load is applied as that used at the bottom of the oven

<div><figure><img src="../.gitbook/assets/applied_temperature_1.png" alt=""><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/applied_temperature_2.png" alt=""><figcaption></figcaption></figure></div>

Caption: Applied temperature is set for area which the heater directly heat, in actual case, the temperature of heater surface is not perfectly evenly distributed, as a simplfying approximation, we treat these areas as isothermal.&#x20;

### Choose Material

Choose corresponding materials in material property setting.

<figure><img src="../.gitbook/assets/simulation_material.png" alt="" width="563"><figcaption></figcaption></figure>

### Heat Loss

To reach a thermal equillibrium state, besides heating source, there must be heat loss. There are two major sources of heat loss.&#x20;

* Convective heat transfer: Heat exchange with its ambient environment which is air in our case
* Natural thermal radiation

### Add Convection Conditions

Convective heat transfer can be \[ref.1]:

* Forced or Assisted convection
* Natural or Free convection

#### Forced or Assisted convection:

Occurs when a fluid flow is induced by an external force, such as a pump, fan or a mixer.

The equation for convection can be expressed as:

$$
q = h_c \, A \, \Delta T
$$

where:

* **q** = heat transferred per unit time\
  &#xNAN;_(W, Btu/hr)_
* **A** = heat transfer area of the surface\
  &#xNAN;_(m², ft²)_
* **h\_c​** = convective heat transfer coefficient of the process _(W/(m²·°C), Btu/(ft²·h·°F)),_ depends on type of media, if its gas or liquid, and flow properties such as velocity, viscosity and other flow and temperature dependent properties
* **ΔT** = temperature difference between the surface and the bulk fluid\
  &#xNAN;_(°C, °F)_

#### Natural or Free convection

Caused by buoyancy forces due to dens ity differences caused by temperature variations in the fluid. At heating the density change in the boundary layer will cause the fluid to rise and be replaced by cooler fluid that also will heat and rise. This continues phenomena is called free or natural convection.

$$
h_{c} = 1.16 \left( 10.45 - v + 10 v^{1/2} \right)
$$

where

v = relative speed between object surface and air (m/s)

h\_c = heat transfer coefficient (W/m^2°K)

This is an empirical equation and can be used for velocities _2_ to _20 m/s_.

#### Choice of Heat Transfer Coefficient

For still air, the heat transfer coefficient (convection value in Fusion setting) is typically below 10 W/m^2°K \[ref.1, ref.2]

#### Selection of Applied Faces&#x20;

The oven is largely enclosed by PEEK insulation, with small gaps between components and a limited surface area directly exposed to ambient air. The airflow conditions within this semi-closed structure are uncertain and may be complex. Therefore, as a simplifying approximation, the convective heat transfer conditions inside and outside the PEEK enclosure are treated as identical. In other words, the same convection coefficient is applied to both internal and external surfaces.

Regarding the ambient temperature configuration, the air temperature in the region between the PEEK insulation and the oven is expected to be higher than that of the external environment. It wold be reasonable to assign a higher ambient temperature to surfaces located inside the PEEK enclosure, including the internal faces of the PEEK and all surfaces of the oven.

{% columns %}
{% column %}
<figure><img src="../.gitbook/assets/convection_copper.png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
<figure><img src="../.gitbook/assets/convection_peek_inner.png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}
{% endcolumns %}

{% columns %}
{% column %}
<figure><img src="../.gitbook/assets/convection_peek_outer_1.png" alt=""><figcaption></figcaption></figure>


{% endcolumn %}

{% column %}
<figure><img src="../.gitbook/assets/convection_peek_bottom.png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}
{% endcolumns %}

Caption: for surfaces inside the PEEK insulation, we treat their convection coefficient same as outside, but with different ambient temperature assignments. Since screw components are missing from the model, inner surfaces of screw holes are excluded.

### Add Radiation Effect

The total radiation intensity of a black body rises as the fourth power of the absolute temperature, as expressed by the [Stefan–Boltzmann law](https://en.wikipedia.org/wiki/Stefan%E2%80%93Boltzmann_law). This radiation effect will dominant when the surface temperature of object is much higher than ambient temperature.

In the general case, the Stefan–Boltzmann law for radiant exitance is given by

$$
M = \varepsilon \sigma T^{4}
$$

where:

* **M** = radiant exitance of the surface _(W/m²)_
* **ε** = emissivity of the emitting surface
* **σ** = Stefan–Boltzmann constant
* **T** = absolute temperature of the surface _(K)_

The emissivity **ε** ranges between 0 and 1. An emissivity of **1** corresponds to an **ideal black body**

For polished copper, emissivity can be 0.023 - 0.052, for Annealed Copper, it can be 0.78. \[ref.3].

Since the copper oven is heated and could be oxidized, value selection from 0.05 to 0.78 should be reasonable.

For PEEK, we could take emissivity as 0.95 \[ref.4]

#### Selection of Applied Faces

This procedure is very similar to the convection setup described previously. Based on the temperature differences experienced by surfaces located inside and outside the PEEK enclosure, different ambient temperatures are assigned accordingly.

{% columns %}
{% column %}
<figure><img src="../.gitbook/assets/radiation_copper.png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
<figure><img src="../.gitbook/assets/radiation_peek_bottom.png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}
{% endcolumns %}

{% columns %}
{% column %}
<figure><img src="../.gitbook/assets/radiation_peek_inner.png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
<figure><img src="../.gitbook/assets/radiation_peek_outer.png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}
{% endcolumns %}

Caption: Following the same rule as in convection process, all surfaces enclosed by PEEK are assigned a higher ambient temperature. Note: different materials require different radiation emissivity values.

### Add Contacts

When simulating the behavior of two components in an assembly under load conditions, we need to define a [**Contact**](https://help.autodesk.com/view/fusion360/ENU/?guid=SIM-CONTACT-TYPES) to describe the interface between them, and thereby cause the bodies to interact appropriately.&#x20;

Here, I am using the automatic contacts with a detection tolerance of 0.1mm.&#x20;

<figure><img src="../.gitbook/assets/automatic_contact_setting.png" alt="" width="375"><figcaption></figcaption></figure>

## Result

As described above, these two parameters are curcial in our simulation

* Convection value with ambient temperature
* Radiation emissivity value with ambient temperature

Through varying these two parameters, we could observe how thermal simulation result changes.

To make notation more concise and clear:

For surfaces enclosed by PEEK, I label the convection value as C\_in and radiation emissivity value as R\_in\_(material).

For surfaces outside of PEEK, I label the convection value as C\_ou, and radiation emissivity value as R\_ou\_(material).&#x20;

A parameter configuration table shown below can be used to schedule the simulation work. values are followed by their ambient temperatures.

<table><thead><tr><th width="93.4765625">Task</th><th>C_in (W/m^2°K + )</th><th>C_ou (W/m^2°K)</th><th>R_in_Oven</th><th>R_in_PEEK</th><th>R_ou_PEEK</th></tr></thead><tbody><tr><td>1</td><td>3+60°C </td><td>3+20°C</td><td>0.3+60°C </td><td>0.95+60°C</td><td>0.95+20°C</td></tr><tr><td>2</td><td>3+40°C </td><td>3+20°C </td><td>0.3+40°C </td><td>0.95+40°C</td><td>0.95+20°C</td></tr><tr><td>3</td><td>8+60°C </td><td>8+20°C </td><td>0.3+60°C </td><td>0.95+60°C</td><td>0.95+20°C</td></tr><tr><td>4</td><td>8+60°C </td><td>8+20°C </td><td>0.7+60°C </td><td>0.95+60°C</td><td>0.95+20°C</td></tr><tr><td>5</td><td>8+40°C </td><td>8+20°C </td><td>0.7+40°C </td><td>0.95+40°C</td><td>0.95+20°C</td></tr></tbody></table>

### Temperature Comparison Along Different Axises

We first examine the thermal distribution across the cross-section of the crystal cavity aperture.

Based on the simulation results from Task 5, the temperatures at three points on the upper rim decrease sequentially from right to left, with a maximum difference of **0.059 °C**. This variation is significantly smaller than the temperature difference observed between points on the upper and lower rims. Therefore, in the next step, we focus on the temperature distribution along the crystal cavity direction.

It should be noted that, as specified in the automatic contact settings, the contact detection tolerance is **0.1 mm**.

<figure><img src="../.gitbook/assets/simulation_direction_comparision.png" alt=""><figcaption></figcaption></figure>



### Temperature Distribution Along Crystal Cavity&#x20;

We pick 6 probe points, 4 of them are at the corners of nonlinear crystal, labeled as from P1 to P6. Shown in the screenshot below.&#x20;

<figure><img src="../.gitbook/assets/simulation_probes.png" alt="" width="563"><figcaption><p>Study temperature distribution along the crystal cavity profile, P5 is chosen because it sits right beneath a smaller cavity, which may see more complex thermal distribution. P1, P3, P4, P6 correspond to four vertices of crystal in this profile</p></figcaption></figure>

Conduct the simulation, repeat these tasks, and take a screenshot for each simulation result

{% columns %}
{% column %}
<figure><img src="../.gitbook/assets/simulation_result_1.png" alt=""><figcaption><p>Result of task 1, with C_in=3+60°C, C_out=3+20°C, R_in_Oven=0.3+60°C, R_in_PEEK=0.95+60°C, R_out_PEEK=0.95+20°C</p></figcaption></figure>
{% endcolumn %}

{% column %}
<figure><img src="../.gitbook/assets/simulation_result_2.png" alt=""><figcaption><p>Result of task 2, with C_in=3+40°C, C_out=3+20°C, R_in_Oven=0.3+40°C, R_in_PEEK=0.95+40°C, R_out_PEEK=0.95+20°C</p></figcaption></figure>
{% endcolumn %}
{% endcolumns %}

{% columns %}
{% column %}
<figure><img src="../.gitbook/assets/simulation_result_3.png" alt=""><figcaption><p>Result of task 3, with C_in=8+60°C, C_out=8+20°C, R_in_Oven=0.3+60°C, R_in_PEEK=0.95+60°C, R_out_PEEK=0.95+20°C</p></figcaption></figure>
{% endcolumn %}

{% column %}
<figure><img src="../.gitbook/assets/simulation_result_4.png" alt=""><figcaption><p>Result of task 4, with C_in=8+60°C, C_out=8+20°C, R_in_Oven=0.7+60°C, R_in_PEEK=0.95+60°C, R_out_PEEK=0.95+20°C</p></figcaption></figure>
{% endcolumn %}
{% endcolumns %}

{% columns %}
{% column %}
<figure><img src="../.gitbook/assets/simulation_result_5.png" alt=""><figcaption><p>Result of task 5, with C_in=8+40°C, C_out=8+20°C, R_in_Oven=0.7+40°C, R_in_PEEK=0.95+40°C, R_out_PEEK=0.95+20°C</p></figcaption></figure>
{% endcolumn %}

{% column %}

{% endcolumn %}
{% endcolumns %}

The detailed simulation values for these points are summarized in the table below.

<table><thead><tr><th width="82.1668701171875">Task</th><th>P1 (°C)</th><th>P2 (°C)</th><th>P3 (°C)</th><th>P4 (°C)</th><th>P5 (°C)</th><th>P6 (°C)</th><th>P0 (°C)</th></tr></thead><tbody><tr><td>1</td><td>119.956</td><td>119.946</td><td>119.942</td><td>119.895</td><td>119.887</td><td>119.889</td><td>119.967</td></tr><tr><td>2</td><td>119.947</td><td>119.934</td><td>119.928</td><td>119.869</td><td>119.859</td><td>119.862</td><td>119.67</td></tr><tr><td>3</td><td>119.933</td><td>119.916</td><td>119.908</td><td>119.828</td><td>119.813</td><td>119.818</td><td>119.951</td></tr><tr><td>4</td><td>119.907</td><td>119.882</td><td>119.871</td><td>119.757</td><td>119.737</td><td>119.743</td><td>119.932</td></tr><tr><td>5</td><td>119.884</td><td>119.853</td><td>119.839</td><td>119.694</td><td>119.668</td><td>119.676</td><td>119.915</td></tr></tbody></table>

And corresponding plot:

<figure><img src="../.gitbook/assets/simluation_temp_plot.png" alt="" width="563"><figcaption></figcaption></figure>

## Conclusion

The simulation results demonstrate that the thermal distribution is sensitive to both convective and radiative heat transfer parameters. The observed trends are physically reasonable; for example, increasing the convective heat transfer coefficient leads to lower surface temperatures.

Among the six probe points considered, **P1** exhibits the highest temperature. **P3** is a critical location for evaluating the temperature gradient along the crystal cavity. **P5** and **P6** experience similarly lower temperatures; therefore, for practical considerations related to thermistor attachment, **P6** is selected as the third thermistor placement location.

### Some further improvements

Since in SHG experiment, the non-linear crystal lies inside the oven, so adding crystal in simulation would be a good improvement to get more trustworthy result for real use scenario.

If there are some methods which can be used to further analyze the properties of material and ambient environment, e.g, the air flow velocity, or the oxidization state of copper oven so that its radiation emissivity value can be better estimated, the simulation could be more trustworthy.

The model we use is a bit different from the real one, a closer model would improve the reliability of simulation result.

As we've discussed, the oven is partially covered by a semi-closed PEEK container, surfaces directly exposed to outside air across a gap in the middle of PEEK definitely experience different convection conditions and radiation ambient temperatures from surfaces at deep corners inside the oven and PEEK. It would be better to split the oven surface into more fine faces and assign them more different and reasonable configurations.&#x20;

Additionally it's not clear how the contact detection tolerance setting affect the simulation result, it would be beneficial to vary this value and study its influence on simulation result.&#x20;

Currently, in the simulation results view in Fusion, the temperature color bar is applied globally to all body components. When the PEEK body is hidden and the temperature distribution on the oven body alone is examined, the color scale remains constrained to the original narrow range. As a result, the temperature variation on the oven body is not rendered intuitively. This issue may be related to software limitations or computater performance.



## Reference

1. &#x20;The Engineering ToolBox (2003). _Understanding Convective Heat Transfer: Coefficients, Formulas & Examples_. \[online] Available at: https://www.engineeringtoolbox.com/convective-heat-transfer-d\_430.html.
2. [https://www.engineersedge.com/heat\_transfer/convective\_heat\_transfer\_coefficients\_\_13378.htm](https://www.engineersedge.com/heat_transfer/convective_heat_transfer_coefficients__13378.htm)
3. Ghane, Mohammad & Ghorbani, Ehsan. (2016). Investigation into the UV-Protection of Woven Fabrics Composed of Metallic Weft Yarns. Autex Research Journal. 16. 10.1515/aut-2015-0021.
4. Properties of Polyetheretherketone (PEEK) transferred materials in a PEEK-steel contact\
   Debashis Puhan1 and Janet S. S. Wong1\*
5. [https://www.engineeringtoolbox.com/emissivity-coefficients-d\_447.html](https://www.engineeringtoolbox.com/emissivity-coefficients-d_447.html)
6. O.C. Zienkiewicz, CBE, FRS. The Finite Element Method: Its Basis and Fundamentals. Sixth edition
7. [https://www.machinedesign.com/additive-3d-printing/fea-and-simulation/article/21832072/whats-the-difference-between-fem-fdm-and-fvm](https://www.machinedesign.com/additive-3d-printing/fea-and-simulation/article/21832072/whats-the-difference-between-fem-fdm-and-fvm)
8. [https://help.autodesk.com/view/fusion360/ENU/?guid=GUID-58159399-B08F-43BF-89E3-57AD489FF29C](https://help.autodesk.com/view/fusion360/ENU/?guid=GUID-58159399-B08F-43BF-89E3-57AD489FF29C)

