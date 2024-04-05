### D. Округление

<table>
 <tr>
    <td>Ограничение времени</td>
    <td>10 секунд</td>
 </tr>
 <tr>
    <td>Ограничение памяти</td>
    <td>1Gb</td>
 </tr>
</table> 


Иногда при округлении к ближайшему всех слагаемых по отдельности получается совсем другая сумма: round(2/3) + round(2/3) + round(2/3) ≠ 2.

Необходимо придумать метод округления, при котором суммарная абсолютная разность элементов двух последовательностей минимальна, а сумма совпадает.

Заданы натуральные числа a<sub>1</sub>, a<sub>2</sub>, …, a<sub>n</sub> и x. Последовательность b<sub>i</sub> определяется по следующей формуле: <span style="white-space: nowrap">$$b_i= \frac{x⋅a_i}{a_1+a_2+…+a_n}$$</span>

Нужно найти последовательность целых чисел c<sub>i</sub> такую, что <span style="white-space: nowrap">$$\sum_{i=1}^{n} |c_i - b_i|$$</span> минимальна и <span style="white-space: nowrap">$$\sum_{i=1}^{n} c_i = x$$</span>  

**Формат ввода**  
В первой строке записаны два целых числа n и x (1 ≤ n ≤ 1000000, 1 ≤ x ≤ 1000000).  
Во второй строке записаны n целых чисел a<sub>1</sub>, a<sub>2</sub>, …, a<sub>n</sub> (1 ≤ a<sub>i</sub> ≤ 1000000).

**Формат вывода** 
Выведите n целых чисел c<sub>1</sub>, c<sub>2</sub>, …, c<sub>n</sub> (0 ≤ c<sub>i</sub> ≤ x). Числа в сумме должны давать значение x.  
Если подходящих последовательностей несколько, выведите любую из них.

<table>
 <tr>
    <th>Тестовые данные</th>
    <th>Ожидаемый результат</th>
 </tr>
 <tr>
    <td>
    	<br>3 2</br>
	<br>1 1 1</br>
    </td>
    	<td>0 1 1</td>
 </tr>
<tr>
	<td>
    		<br>5 1</br>
		<br>6 2 3 4 5</br>
  	</td>
  	<td>1 0 0 0 0</td>
  </tr>
  
<tr>
    <td>
      	<br>3 40</br>
	<br>100 12 23</br>
    </td>
    <td>
      30 3 7
    </td>
</tr>
	
 <tr>
		<td>
		<br>6 21</br>
	<br>1 2 3 4 5 6</br>
    </td>
    <td>
      1 2 3 4 5 6
    </td>
  </tr>
	
<tr>
    <td>
      <br>1 13</br>
<br>17</br>
    </td>
    <td>
      13
    </td>
</tr>
</table>  

```c++
#include <algorithm>
#include <iostream>
#include <vector>
#include <string>
#include <unordered_set>

using namespace std;

int main(int argc, char const *argv[])
{
    int n, x; cin >> n >> x;
    vector<int> a(n);
    long long aSum = 0;
    for(int i = 0; i < n; i++){
        cin >> a[i];
        aSum += a[i];
    } 
    vector<long double> b(n);
    vector<long long> bR(n);
    long long brSum = 0;
    for(int i = 0; i < n; i++) {
        b[i] = (long double) x * a[i] / aSum;
        bR[i] = (long long) b[i];
        brSum += bR[i];
    }
    long long diff = x - brSum;
    vector<pair<long double, int>> fractionalParts;
    for (size_t index = 0; index < b.size(); index++) {
        fractionalParts.emplace_back(b[index] - (long double)bR[index], index);
    }
    sort(fractionalParts.begin(), fractionalParts.end(), greater<>());

    for (int i = 0; i < diff; i++) {
        bR[fractionalParts[i].second]++;
    }
    for (int value : bR) {
        cout << value << " ";
    }
    return 0;
}
```
