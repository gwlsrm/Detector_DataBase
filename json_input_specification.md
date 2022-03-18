# Спецификация входных json-файлов

**версия 1.0**

## 1. Описание формата

Описание моделей (детекторы, источники, анализаторы, коллиматоры, параметры для расчёта) для расчётных модулей в формате json.

В корне json ключ -- тип модели: "Detector", "Source", "Analyzer", "Collimator", "CalculationParameters", "ContainerSource", значение -- объект (словарь).



## 2. Описание модели детектора

У объекта должен быть тип `Type`: `"Type": "DetectorType"`, где DetectorType: `COAXIAL`, `SCINTILLATOR`, `COAX_WELL`. 

Размеры указываются в секции (объекте) "Geometry": `"LayerName": double_value`.

Материалы указываются в объекте "Material": для каждого слоя указывается материал:

```json
"LayerName": {
	"rho": double_value,
    "fraction_type": "MASS",
    "elements": [
        {
            "z": int_value,
            "frac": double_value
        }
    ]
}
```



## 3. Описание модели источника

Тип источника: `POINT`, `CYLINDER`, `MARINELLI`, `CONE`.

Описание секции размеров и материалов аналогичны формату детектора