# 電感電容測量表
![LC meter](https://github.com/SuperRockManZero/LC-meter/blob/main/Photo/LC_meter_B5_p10.jpg)
此專案使用 PIC16F648A 微處理器製作一個電感電容測量表。依據 LC 諧振原理，並使用已知的參考電容計算出待測量的電感或電容數值。

## 專案介紹

電感電容測量表是一個電子測量工具，用來測量電感$`(L)`$和電容$`(C)`$精確數值。該工作原理是利用 LC 諧振特性並測量其震盪頻率，並通過已知的參考電容或電感依據諧振公式計算出待測量的電感或電容。

## 特性
- 使用 PIC16F648A 微處理器
- 基於 LC 諧振原理測量頻率
- 通過已知的參考電容或電感計算出電感或電容數值
- 顯示測量結果
- 測量範圍：
  - 電容：$`1pF \sim 0.97uF`$
  - 電感：$`100nH \sim 97mH`$
  - 誤差：$`\pm1\%`$

誤差值$`\pm1\%`$是取決於電路上一顆作為校準用電容的精密度，另外在程式計算過程中包含了浮點運算誤差，實際誤差將會稍大於$`\pm1\%`$。

## 原理說明
![精密電容](https://github.com/SuperRockManZero/LC-meter/blob/main/Photo/P_20160116_233028_HDR.jpg)

#### *LC 諧振電路*：
LC 諧振電路由電感$`(L)`$和電容$`(C)`$組成，當電路處於諧振狀態時，電路的諧振頻率率$`(f)`$可以通過以下公式計算：

$$f = \frac{1}{2\pi\sqrt{LC}}$$
  
#### *校準*：
因為實體電路存在許多未知的雜散電容與電感，這將會造成量測上的誤差。使用一高精密度電容作為校準雜散電容與電感造成的誤差因素。$`C_{cal}`$為校準用的精密電容，設$`C_{3}`$與雜散電容總合為$`C`$，$`L_{1}`$與雜散電感總合為$`L`$。

![LC osc schematic](https://github.com/SuperRockManZero/LC-meter/blob/main/Photo/LC_Oscillator_2-Schematic.jpg)
*圖一：LC諧振電路*

當 $`SW`$ 斷開時，其諧振頻率為 $`f_{1}`$：

$$f_{1} = \frac{1}{2\pi\sqrt{LC}}$$

當 $`SW`$ 閉合時，$`C_{cal}`$與$`C`$並聯(含雜散電容)，其諧振頻率為 $`f_{2}`$：

$$f_{2} = \frac{1}{2\pi\sqrt{L(C+C_{cal})}}$$

使$`\frac{f_{1}}{f_{2}}`$並化簡公式。

$$\frac{f_{1}}{f_{2}}=\frac{\frac{1}{2\pi\sqrt{LC}}}{\frac{1}{2\pi\sqrt{L\left( C+C_{cal} \right)}}}=\frac{\sqrt{L\left( C+C_{cal} \right)}}{\sqrt{LC}}$$

$$\Rightarrow \frac{f_{1}^{2}}{f_{2}^{2}}=1+\frac{C_{cal}}{C}$$

由上式可得知：$`C_{3}`$與雜散電容總合$`C`$：

$$C=\frac{C_{cal}}{\frac{f_{1}^{2}}{f_{2}^{2}}-1}$$

且電感$`L`$的數值與公式無關。

#### *量測電容*：
被測量的電容為$`C_{x}`$，在電路上與 $`C`$ 並聯。($`C`$是$`C_{3}`$與雜散電容並聯後的等效電容)

![測量電容](https://github.com/SuperRockManZero/LC-meter/blob/main/Photo/LC_Oscillator_3a-Schematic.jpg)
*圖二：量測電容*

代入諧振公式，求其諧振頻率為$`f_{2}`$：

$$f_{2}=\frac{1}{2\pi\sqrt{L\left( C+C_{x} \right)}}$$

取校準過程中的$`f_{1}`$。

同樣使 $`\frac{f_{1}}{f_{2}}`$ 並化簡公式。

$$\frac{f_{1}}{f_{2}}=1+\frac{C_{x}}{C}$$

$$\Rightarrow C_{x}=C\left(\frac{f_{1}^{2}}{f_{2}^{2}}-1 \right)$$

同樣的在公式與$`L`$無關，且在先前的校準後已得知 $`f_{1}`$與$`C`$。經公式計算後可求得被測量的電容$`C_{x}`$。

#### *量測電感*：
被測量的電感為$`L_{x}`$，在電路上與$`L`$串聯。($`L`$是$`L_{1}`$與雜散電感的串聯後的等效電感)

![量測電感](https://github.com/SuperRockManZero/LC-meter/blob/main/Photo/LC_Oscillator_3b-Schematic.jpg)
*圖三：量測電感*

代入諧振公式求其諧振頻率為$`f_{2}`$：

$$f_{2}=\frac{1}{2\pi\sqrt{\left(L+L_{x} \right)C}}$$

整理公式為$`formula 1`$：

$$L+L_{x}=\frac{1}{C\left(2\pi f_{2} \right)^{2}}$$

取先前校準過程中的$`f_{1}`$，並整理公式為：

$$L=\frac{1}{C\left(2\pi f_{1} \right)^{2}}$$

將$`L`$帶入$`formula 1`$，並化簡公式：

$$\frac{1}{C\left(2\pi f_{1} \right)^{2}}+L_{x}=\frac{1}{C\left( 2\pi f_{2}\right)^{2}}$$

$$\Rightarrow L_{x}=\left( \frac{1}{f_{2}^{2}}-\frac{1}{f_{1}^{2}} \right)\frac{1}{C\left( 2\pi \right)^{2}}$$

同樣的在公式與$`L`$無關，且在先前的校準後已得知$`f_{1}`$與$`C`$。經公式計算後可求得被測量的電感$`L_{x}`$。
另外相同於測量電容的方法，也可以解出測量電感的公式：

$$\frac{f_{1}^2}{f_{2}^2}=1+\frac{L_{x}}{L}$$

$$\Rightarrow L_{x}=\left( \frac{f_{1}^2}{f_{2}^2}-1 \right)L$$

唯；這樣的公式與$`L`$有關，所以不採用此公式。

## 硬件組成

- PIC16F648A 微處理器
- LC 諧振電路
- 參考電容
- 1602 文字型顯示器
- 9V電源及5V穩壓電路

## 電路圖
![LC meter schematic](https://github.com/SuperRockManZero/LC-meter/blob/main/Photo/LC_Meter_ver_B2.jpg)

- 校準電容：$`C5`$為$`\pm 1\%`$精密電容用於電路校準。
- Mcu：PIC16F648A，以其內建的比較器做為諧振電路。
- 1602 文字型顯示器：16字2行，符合或相容KS0066驅動控制器。

## 程式設計

#### **計數與計時**：
LC 諧振週期數為$`(T)`$，時間長為$`(t)`$，頻率為$`(f)`$

$$f=\frac{T}{t}$$

- **計數**：使用Mcu內建 TIMER0 (8bits) 作為計數LC諧振周期，將預分頻器(Prescaler)設為1：256。所以最多可計數：$`2^{8}\times2^{8}=65536`$
- **計時**： 使用Mcu內建 TIMER1 (16bits) 作為計時器，預分頻器(Prescaler)設為1：1，並且設一8bits的變數作為計數 TIMER1 溢位次數。TIMER1的時鐘速度為 4MHz / 4，故；可計時的最長時間為：$`2^{16}\times2^{8}\times\frac{4}{4MHz}=16.777216sec`$(實際上只需用到約4.2sec)
- **TIMER0 與 TIMER1**：如下圖。
![Timer0與Timer1](https://github.com/SuperRockManZero/LC-meter/blob/main/Photo/timing3.jpg)
$$T_{STOP} - T_{START} = \text{LC週期時間}\times{65536}\left(usec\right)$$

#### **改寫諧振公式**：
現將頻率換算為週期時間($`sec`$)並改寫公式。

設：$`f_{1}`$的週期時間為$`t_{1}`$，$`f_{1}=\frac{1}{t_{1}}`$

設：$`f_{2}`$的週期時間為$`t_{2}`$，$`f_{2}=\frac{1}{t_{2}}`$

帶入以下公式。
- $`formula (a)`$：
  
  $$C=\frac{C_{cal}}{\frac{f_{1}^{2}}{f_{2}^{2}}-1}\Rightarrow C=\frac{C_{cal}}{\frac{t_{2}^{2}}{t_{1}^{2}}-1}$$
- $`formula (b)`$：

  $$C_{x}=\left(\frac{f_{1}^{2}}{f_{2}^{2}}-1\right)C\Rightarrow C_{x}=\left({\frac{t_{2}^{2}}{t_{1}^{2}}-1}\right)C$$
- $`formula (c)`$：

  $$L_{x}=\left( \frac{1}{f_{2}^{2}}-\frac{1}{f_{1}^{2}} \right)\frac{1}{C\left( 2\pi \right)^{2}}$$
$$\Rightarrow L_{x}=\left({t_{2}^{2}}-{t_{1}^{2}} \right)\frac{1}{C\left( 2\pi \right)^{2}}$$

#### **技巧**：
設$`T_{1}`$是$`t_{1}`$的65536個週期時間，$`t_{1}=\frac{T_{1}\times{10^{-6}}}{65536}`$
設$`T_{2}`$是$`t_{2}`$的65536個週期時間，$`t_{2}=\frac{T_{2}\times{10^{-6}}}{65536}`$
註：$`T_{1}`$與$`T_{2}`$的單位是$`usec`$

帶入$`formula (a)(b)(c)`$，並化簡。
- $`formula (a)`$ 改寫為 $`formula (a1)`$

  $$C=\frac{C_{cal}}{\frac{T_{2}^{2}}{T_{1}^{2}}-1}$$
- $`formula (b)`$ 改寫為 $`formula (b1)`$

  $$C_{x}=\left({\frac{T_{2}^{2}}{T_{1}^{2}}-1}\right)C$$
- $`formula (c)`$ 改寫為 $`formula (c1)`$

  $$L_{x}=\left({T_{2}^{2}}-{T_{1}^{2}} \right)\frac{1}{C\left(65536\times {2\pi}\right)^{2}}$$
  
#### **浮點運算**：
由於二進制浮點數(float)不能表達所有的實數，只能表示最接近的可表達數。浮點數及運算可能存在誤差，但這些誤差量必定趨於最小。例如：將某一存在誤差的二進制浮點數轉為十進制，其誤差部分必定在於十進制數的最右邊。
所以；$`formula (a1)`$與$`formula (b1)`$必須再改寫，期使減少浮點運算的誤差。
- $`formula (a1)`$改寫為$`formula (a2)`$

  $$C=\frac{C_{cal}}{\frac{T_{2}^{2}}{T_{1}^{2}}-1}\Rightarrow C=\frac{C_{cal}}{\frac{T_{2}^{2}-T_{1}^{2}}{T_{1}^{2}}}$$
- $`formula (b1)`$改寫為$`formula (b2)`$

  $$C_{x}=\left({\frac{T_{2}^{2}}{T_{1}^{2}}-1}\right)C\Rightarrow C_{x}=\left({\frac{T_{2}^{2}-T_{1}^{2}}{T_{1}^{2}}}\right)C$$
  
#### **常數**：
$`formula (c1)`$的尾部是一段常數。這常數可預先計算帶入算式，這樣可以免去一些Mcu運算工作。
設常數為$`ARG3`$
$$ARG3=\frac{10^{-6}}{\left(65536\times 2\pi\right)^{2}}\approx 5.898\times 10^{-18}$$
- $`formula (c1)`$改寫為$`formula (c2)`$

  $$L_{x}=\left(T_{2}^{2}-T_{1}^{2} \right)\times\frac{ARG3}{C}$$
  
#### **減少Mcu運算**：
$`formula (a2)`$、$`(b2)`$ 與 $`(c2)`$ 中的 $`T1`$、$`ARG3`$與$`C`$，均在校準時可得知。為免去後續量測時重覆計算相同的運算，故將部份公式修改如下。
- 設常數：
  - $`ARG1=T_{1}^{2}`$
  - $`ARG2=\frac{ARG3}{C}`$
  - $`ARG3=5.898\times{10^{-18}}`$
- $`formula (a2)`$改寫為$`formula (a3)`$

  $$C=\frac{C_{cal}}{\frac{T_{2}^{2}-ARG1}{ARG1}}$$
- $`formula (b2)`$改寫為$`formula (b3)`$

  $$C_{x}=\left({\frac{T_{2}^{2}-ARG1}{ARG1}}\right)C$$
- $`formula (c2)`$改寫為$`formula (c3)`$

  $$L_{x}=\left(T_{2}^{2}-ARG1 \right)\times{ARG2}$$

## 使用說明

1. **燒錄程序**：
   - 使用 PIC 燒寫器將 [hex file](https://github.com/SuperRockManZero/LC-meter/blob/main/Hex/LC_Meter_ver_B5.production.hex) 燒錄到 PIC16F648A 微處理器。

2. **操作步驟**：
   - 打開電源，啟動測量表。
   - 設定切換開關，選擇測量電感或電容。
   - 將待測電感或電容接入電路，系統將自動測量頻率並計算出電感或電容的數值。
   - 在顯示屏上查看測量結果。

## 注意事項
- 使用高精度的參考電容，以提高測量準確性。

## 貢獻
如果你有改進建議或發現問題，請提交 Issues 或 Pull Requests。

## 授權
此專案使用 [MIT 授權](LICENSE)。
