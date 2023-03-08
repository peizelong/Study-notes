# PBR Function in realtime rendering实时渲染中的PBR方程

$$
L_o(p,\omega_o)=\int_\Omega f_r(p,\omega_i,\omega_o)L_i(p,\omega_i)n\cdot \omega_i d\omega_i\\=\int_\Omega(k_d\frac{c}{\pi}+k_s\frac{DFG}{4(\omega_o\cdot n)(\omega_i\cdot n)})L_i(p,\omega_i)(\omega_i \cdot n)d\omega_i
$$

$$
L_o(p,\omega_o)=\int_\Omega f_r(p,\omega_i,\omega_o)L_i(p,\omega_i)n\cdot \omega_i d\omega_i
$$

L ：辐射率(Radiance) ω：立体角 (将立体角和面积看作无穷小后就变成了一个方向向量和一个点)

$ω_o$ ：射出方向(观察方向)    $ω_i$ :射入方向(光源方向)

$L_o(p,\omega_o)$：从$\omega_o$方向上观察 光线投射到点p反射出来的 Irradiance

$\int_\Omega.....d\omega_i$：半球Ω的积分

### Cook-Torrance BRDF方程

$$
f_r=k_df_{lambert}+k_sf_{cook-torrance}
$$

$k_d$ 被折射部分的能量占比 $k_s$ 被反射部分能量占比

$$
f_{lambert}=\frac{c}{\pi} \\漫反射方程
$$

c：纹理颜色

$$
f_{cook-torrance}(l,v)=\frac{D(h))F(v,h))G(l,v,h))}{4(n\cdot l)(n\cdot v))} \\镜面反射方程
$$

#### D：Normal Distribution Function (法线\正态分布函数)

从统计学上近似表示某些(半程)向量h取向一直的微平面的比率

$$
NDF_{GGXTR}(n,h,\alpha)=\frac{\alpha^2}{\pi((n\cdot h)^2(\alpha^2-1)+1)^2} \\法线/正态分布函数
$$

h：微平面上用来比较的半程向量  α：表面粗糙度

#### F：Fresnel Rquation 菲涅尔方程

被反射的光线对比光线被折射的部分所占的比率，这个比率会随着我们观察的角度不同而不同

$$
F_{Schlick}(h,v,F_0)=F_0+(1-F_0)(1-(h\cdot v))^5\\菲涅尔方程
$$

$F_0$：平面的基础反射率

菲涅尔方程仅对非金属表面有定义，对于金属表面不能得出正确的结果，为了使用同一种菲涅尔方程对金属表面来进行计算所以需要引入一个金属度(Metalness)参数来描述材质表面是金属还是非金属

所以$F_0$ 计算方法一般为

$$
F_0= vec3(0.04);
F_0=mix(F_0,surfaceColor.rbg,metalness);
$$

#### G：Gemoetry Function几何函数

$$
G_{SchlickGGX}(n,v,k)=\frac{n\cdot v}{(n\cdot v)(1-k)+k}\\几何函数
$$

$$
K_{direct}=\frac{(\alpha+1)^2}{8} \qquad K_{IBL}=\frac{\alpha^2}{2}\\K的两种计算
$$

史密斯法：

$$
G(n,v,l,k)=G_{sub}(n,v,k)G_{sub}(n,l,k)
$$

将Schlick-GGC作为G$sub$

n:法线   v:观察方向   l:光源方向   k:k是α的重映射(取决于直接光照还是IBL光照)
