# Классификация изображений цифр банковской карты

В данной работе для распознавания символов была разработана
свёрточная нейронная сеть, состоящая из 6 слоев.
Входной слой состоит из 3-х карт признаков, которые соответствуют
трем цветовым каналам распознаваемого изображения, и содержит 60 * 40 * 3
= 7200 нейронов, где первые два параметра складываются из размерности
изображения. Данный слой служит для передачи нейронной сети входной
информации.

Следующий слой является первым скрытым свёрточным слоем. Он
извлекает элементы, которые могут содержать в себе информацию, важную
для дальнейшей классификации, из исходного изображения путем его
«сканирования» с помощью фильтра 3 × 3 пикселей. Для каждой такой области
пикселей в изображении операция свертки вычисляет скалярное произведение
значений пикселей изображения и весов, определенных в фильтре. По мере
обучения модели значения весов будут изменяться.
Далее следуют ещё два скрытых свёрточных слоя с фильтрами
размерностью 5 × 5 и 7 × 7 пикселей соответственно.

В качестве функции активации для свёрточных слоев эмпирическим
путем была выбрана функция ReLU. Выбор осуществлялся на основании
достижения необходимого уровня ошибки в процессе обучения. Процентная
величина, отражающая расхождение между ожидаемым и полученным
ответами, полученная в результате использования данной функции, была
значительно ниже по сравнению со значениями других функций.

Следующим слоем является слой выравнивания. Он преобразует
двумерный массив признаков в одномерный, не имеет параметров для
обучения, служит для переформатирования данных и передаёт их на вход
следующему полносвязному слою – слою пулинга. Он, в свою очередь,
уплотняет карту признаков до числа, равного количеству классов. Для
выходного слоя используется функция активации Softmax.
Таким образом, для каждого распознаваемого изображения символа на
выходе получаем массив псевдовероятностных оценок нахождения данного
символа на данном изображении

Для контроля скорости обучения на этапе компиляции модели в качестве
оптимизатора был выбран стохастический градиентный спуск (SGD) с
категориальной кросс-энтропией как функцией потери. Для обучения сети
использовался алгоритм обратного распространения ошибки. В качестве
оценки точности распознавания при валидации значений использовалась
метрика «accuracy».

Ответ модели машинного обучения представляет собой массив из 38
значений – по количеству классов, на которые была разбита обучающая
выборка. Каждое значение лежит в интервале от 0 до 1 и характеризует
уверенность модели в том, что переданное на вход изображение принадлежит
тому или иному классу.

Потому при обработке результата предсказания от модели машинного
обучения необходимо пройтись по всему вектору, возвращаемому моделью, и
найти индекс элемента с максимальным значением. Искомый индекс будет
соответствовать порядковому номеру того класса, который был определен при
обучении модели и которому соответствует символ, предсказанный моделью.
