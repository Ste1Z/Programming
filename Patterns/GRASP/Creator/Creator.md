Назад к: [[GRASP]]

---
**Creator** или **_Создатель_** — суть ответственности такого объекта в том, что он создает другие объекты. Сразу напрашивается аналогия с **абстрактной фабрикой**. 
По сути шаблон проектирования Абстрактная фабрика (создание объектов концентрируется в отдельном классе) это альтернатива создателя.
Но есть ряд моментов, которые должны выполняться, когда мы наделяем объект ответственностью **создателя**:
- Создатель содержит или агрегирует создаваемые объекты;
- Создатель использует создаваемые объекты ;
- Создатель знает, как проинициализировать создаваемый объект ;
- Создатель записывает создаваемые объекты
- Создатель имеет данные инициализации для A