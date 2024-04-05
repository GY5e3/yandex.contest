### A. Тетрамино

<table>
 <tr>
    <td>Ограничение времени</td>
    <td>1 секунда</td>
 </tr>
 <tr>
    <td>Ограничение памяти</td>
    <td>256Mb</td>
 </tr>
</table>  

На шахматном поле 8×8 некоторые клетки пустые, а некоторые заняты фигурами. Определите количество способов разместить тетрамино на этом поле, чтобы фигура занимала целиком четыре свободные клетки. В задаче мы рассматриваем тетрамино только в форме буквы Т.

**Формат ввода**  
Входные данные состоят из 8 строк по 8 символов. Пустая клетка задается точкой (‘.’), а занятая звездочкой (‘*’).

**Формат вывода**  
Выведите количество способов разместить тетрамино на поле.

<table>
 <tr>
    <th>Тестовые данные</th>
    <th>Ожидаемый результат</th>
 </tr>
 <tr>
    <td>
    <br>....****</br>
    <br>....****</br>
    <br>....****</br>
    <br>....****</br>
    <br>****....</br>
    <br>****....</br>
    <br>****....</br>
    <br>****....</br>
    </td>
    <td>48</td>
 </tr>
  <tr>
<td>
    <br>********</br>
<br>********</br>
<br>********</br>
<br>********</br>
<br>********</br>
<br>********</br>
<br>********</br>
<br>********</br>

  </td>
  <td>0</td>
  </tr>
  <tr>
    <td>
      <br>********</br>
<br>********</br>
<br>********</br>
<br>********</br>
<br>********</br>
<br>********</br>
<br>******..</br>
<br>........</br>
    </td>
    <td>
      1
    </td>
  </tr>
  
</table>  

```c++
#include <iostream>
#include <vector>
#include <string>
#include <unordered_set>

using namespace std;

struct PairHash
{
    template <class T1, class T2>
    std::size_t operator()(const std::pair<T1, T2> &p) const
    {
        auto h1 = std::hash<T1>{}(p.first);
        auto h2 = std::hash<T2>{}(p.second);
        return h1 ^ h2;
    }
};
int main(int argc, char const *argv[])
{
    int n = 8;
    unordered_set<pair<int,int>, PairHash> st;
    for(int i = 0; i < n; i++) {
        string inputLine;
        cin >> inputLine;
        for(int j = 0; j < n; j++) {

            if(inputLine[j] == '.') st.insert({i, j});
        }
    }
    int count = 0;
    for(auto it = st.begin(); it != st.end(); it++) {
        if(st.find({it->first - 1, it->second}) != st.end() 
        && st.find({it->first, it->second - 1}) != st.end()
        && st.find({it->first + 1, it->second}) != st.end()) {
            count++;
        }
        if(st.find({it->first - 1, it->second}) != st.end() 
        && st.find({it->first, it->second - 1}) != st.end()
        && st.find({it->first, it->second + 1}) != st.end()) {
            count++;
        }
        if(st.find({it->first + 1, it->second}) != st.end() 
        && st.find({it->first, it->second - 1}) != st.end()
        && st.find({it->first, it->second + 1}) != st.end()) {
            count++;
        }

        if(st.find({it->first - 1, it->second}) != st.end() 
        && st.find({it->first + 1, it->second}) != st.end()
        && st.find({it->first, it->second + 1}) != st.end()) {
            count++;
        }
    }
    cout << count;
    return 0;
}
```
