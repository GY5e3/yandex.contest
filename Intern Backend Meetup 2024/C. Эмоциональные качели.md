### C. Эмоциональные качели

<table>
 <tr>
    <td>Ограничение времени</td>
    <td>2 секунды</td>
 </tr>
 <tr>
    <td>Ограничение памяти</td>
    <td>256Mb</td>
 </tr>
</table>  

Популярный блоггер выяснил, что шок-контент уже не так сильно привлекает зрителей. Теперь чтобы видео стало популярным надо как следует раскачать эмоциональные качели. Эмоциональная окраска каждого события характеризуется буквой английского алфавита от максимально позитивного (a), до максимально негативного (z).

Чтобы определить эмоциональность качель в отрезке подряд идущих событий, необходимо выбрать максимально негативное и максимально позитивное событие на этом отрезке и посчитать разницу порядковых номеров букв в алфавите. Например, если на отрезке максимально позитивное событие "b", а максимально негативное — "e", то величина эмоциональности качель будет равна трём, т.к. "e" стоит на пятом месте в алфавите, а "b" — на втором. Блоггер выписал эмоциональную окраску всех событий за последнее время в том порядке, как они происходили. Теперь он хочет выбрать последовательность подряд идущих событий с максимальной эмоциональностью качель, а если их несколько — выбрать первую из самых коротких таких последовательностей (клиповое мышление зрителей мешает осмыслять длинные видео).

**Формат ввода**  
Вводится непустая строка из прописных английских букв, ее длина не превышает 2 × 105.

**Формат вывода**  
Выведите искомую последовательность подряд идущих событий. Если таких несколько — выведите ту, которая встречается раньше всего.

<table>
 <tr>
    <th>Тестовые данные</th>
    <th>Ожидаемый результат</th>
 </tr>
 <tr>
    <td>aba</td>
    <td>ab</td>
 </tr>
  <tr>
    <td>zzz</td>
    <td>z</td>
  </tr>
</table>  

**Примечания**  
В первом примере любой отрезок длины 1 (подстрока из одной буквы) будет иметь величину эмоциональности качель, равную нулю. В подстроках длины 2 (ab и ba) максимальная эмоциональность равна единице, но подстрока ab встречается раньше. Величина эмоциональности качель для всей строки aba также равна единице, но эта строка длиннее, чем ab.  
Во втором примере величина эмоциональности качель во всех подстроках равна нулю, поэтому выбирается самая короткая строка (длины 1), а среди таких строк — самая левая (но в примере все они одинаковы).

```c++
#include <iostream>
#include <vector>
#include <string>

using namespace std;

int binarySearchClosest(const vector<int> &arr, int target)
{
    int left = 0;
    int right = arr.size() - 1;

    while (left <= right)
    {
        int mid = left + (right - left) / 2;

        if (arr[mid] == target)
        {
            return mid;
        }

        if (arr[mid] < target)
        {
            left = mid + 1;
        }
        else
        {
            right = mid - 1;
        }
    }

    if (left > right) {
        if (right >= 0 && abs(arr[right] - target) <= abs(arr[left] - target)) {
            return right;
        } else {
            return left;
        }
    }
    return -1;
}
int main(int argc, char const *argv[])
{
    string data;
    cin >> data;

    int indexMaxLeft = 0;
    int maxN = 0;
    int minN = 30;

    vector<int> maxi;
    vector<int> mini;

    for (int i = 0; i < data.length(); i++)
    {
        if (maxN < data[i] - 'a')
        {
            maxi.clear();
            maxN = data[i] - 'a';
            maxi.push_back(i);
        }
        else if (maxN == data[i] - 'a')
        {
            maxi.push_back(i);
        }
        if (minN > data[i] - 'a')
        {
            mini.clear();
            minN = data[i] - 'a';
            mini.push_back(i);
        }
        else if (minN == data[i] - 'a')
        {
            mini.push_back(i);
        }
    }
    int f = 0, s = data.size();
    for (int i = 0; i < maxi.size(); i++)
    {
        int index = binarySearchClosest(mini, maxi[i]);
        if (abs(mini[index] - maxi[i]) < abs(f - s))
        {
            f = mini[index];
            s = maxi[i];
        }
    }
    if (f <= s)
        string sub = data.substr(f, s - f + 1);
    else
        string sub = data.substr(s, f - s + 1);
    cout << sub;
    return 0;
}
```
