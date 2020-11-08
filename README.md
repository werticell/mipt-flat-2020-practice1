# First formal languages practicum task
**Задача 10** Даны регулярное выражение `α` в обратной польской записи, буква `x` и натуральное число `k`. Вывести длину кратчайшего слова из
языка `L`, содержащего суффикс `x^k`.


## Solution 

Будем использовать стэк и постепенно обрабатывать выражение. В стеке будем поддерживать наши текущие состояния.
Состояние – это класс `State` с двумя полями:
1. `min_word_length_` – минимальная длина возможного слова для данного состояния;
2. `min_word_len_with_suffix_` – массив длины `k`, который для индекса `i` хранит минимальную длину, 
которое заканчивается на суффикс `x^i`, либо `INF`, если такого слова нет для данного состояния.

**Пересчитываем состояния** мы исходя из следующих правил:
1. Если это буква, то мы добавляем ее и устанавливаем `min_word_len_with_suffix_[1] = 1`;
2. Если это `EPSILON` (т.е. `1`), то мы добавляем ее и устанавливаем `min_word_len_with_suffix_[0] = 0`;
3. Если это `+`, то в качестве `min_word_length_` для нового результирующего состояния мы берем минимальное из двух значений операндов,
 в массиве также для каждого `i` обновляем минимумом из значений `min_word_len_with_suffix_[i]` обоих операндов.
4. Если это `.`, то в качестве `min_word_length_` для нового результирующего состояния мы берем сумму двух значений операндов.
В массиве сначала обновляем значения для результата как значение `right.min_word_len_with_suffix_[i] + left.min_word_length_`, 
потом для тех элементов, для которых `x[i] == i`, что значит что для правого состояния было слово минимальной длины, 
которое полностью состояло из нужных нам букв, такое слово можно попытаться продлить суффиксами левого слова, чтобы получить в 
итоге более длинные суффиксы и меньшие значения в массиве результата для них (обновляем через минимум того, что там уже лежало, 
и суммой `right.min_word_len_with_suffix_[i] + left.min_word_len_with_suffix_[j]`);
5. Если это `*`, то `min_word_length_ = 0` для результирующего состояния, а массив не меняется, кроме случаев, 
когда можно обновить через те значения, где `x[i] == i` и получить меньшие результаты для всех `j` кратных `i` меньших `k`.

**Асимптотика** O(|α| * k^2), так как операция над двумя состояниями происходит в худшем случае за O(k^2) и происходит не больше раз чем длина регулярного выражения.


### Building and testing
You will need [googletest](https://medium.com/@alexanderbussan/getting-started-with-google-test-on-os-x-a07eee7ae6dc) 
to build and test it.

To build project. (Choose bash or zsh)
````
    git clone <github link>
    cd mipt-flat-2020-practice1
    bash(or zsh) build.sh
````

To run tests after building. (You can write your own tests in main_test.cpp.)
````
    cd bin
    ./Test
````
To run program after building.
````
    cd bin
    ./FormalLanguagesPracticum
````

 