# Спецификация входных json-файлов

**версия 1.0**

## 1. Описание формата

Описание моделей (детекторы, источники, анализаторы, коллиматоры, параметры для расчёта) для расчётных модулей в формате json.

В корне json ключ -- тип модели: "Detector", "Source", "Analyzer", "Collimator", "CalculationParameters", "ContainerSource", значение -- объект (словарь).



## 2. Описание модели детектора (Detector)

У объекта должен быть тип `Type`: `"Type": "DetectorType"`, где DetectorType: `COAXIAL`, `SCINTILLATOR`, `COAX_WELL`. 

Размеры указываются в секции (объекте) `"Geometry": "LayerName": double_value`.

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



## 3. Описание модели источника (Source)

Тип источника (`Type`): `POINT`, `CYLINDER`, `MARINELLI`, `CONE`.

Описание секции размеров (`Geometry`) и материалов (`Material`) аналогичны формату детектора



## 4. Описание модели анализатора (Analyzer)

Модель анализатора имеет фиксированный набор полей:

```json
"Name": "DSpec_8K_3Mev",
"FWHM_122": 0.77,
"FWHM_662": 1.26,
"FWHM_1332": 1.73,
"kev_per_ch": 0.3662,
"N_ch": 8192
```

- `FWHM_122`, `FWHM_662`, `FWHM_1332` -- значения FWHM для энергий 122, 662 и 1332 кэВ, должно быть указано минимум одно из полей.

- `kev_per_ch` -- цена канала (обязательное поле)

- `N_ch` -- число каналов (обязательное поле)
- `Name` -- имя анализатора (необязательное поле)



## 5. Описание модели коллиматора (Collimator)

Тип коллиматора (`Type`): `TYPE1`, `TYPE2`.

Описание секции размеров (`Geometry`) и материалов (`Material`) аналогичны формату детектора



## 6. Описание модели источников в контейнере (ContainerSource)

Содержит секцию `Cells`, в которой массив слоёв. Необязательное поле `Name`. И поле `DetectorPosition`.

Каждый слой состоит из полей:

-  `Shape` (возможные значения: "CylZ", "Cuboid", "Sphere", "Point"),
- `ParentIdx` -- индекс родительского слоя (-1 -- если слой корневой -- нет родительского слоя),
- `ChildIndex` -- массив индексов дочерних (вложенных) слоёв,
- `Dimensions` -- размеры слоя в см, содержит поля `Dim1`, `Dim2`, ...
- `Position` -- положение центра слоя (`x`, `y`, `z`) в см, центр координат в центре корневого слоя,
- `Material` -- материал слоя
  - `rho` -- плотность материала,
  - `elements` -- массив элементов (`z`, `frac`)

`DetectorPosition` -- положение центра детектора относительно центра источника

	- `detX`, `detY`, `detZ` -- координаты центра детектора в см,
	-  `detPhi`, `detTheta` -- направление детектора в радианах



## 7. Описание параметров расчёта (CalculationParameters)

Секция CalculationParameters имеет набор полей:

```json
"low_energy_threshold": 0.05,
"pair_peaks_threshold": 1.5,
"xray_escape_threshold": 0.3,
"photon_LEcutoff": 0.001,
"electron_LEcutoff": 0.001,
"phys_spectrum_Emax": 1.5,
"phys_spectrum_dE": 0.01,
"useGLECS": false,
"useEPDL97": false,
"calc_electron_ttb": false,
"calc_electron_real": false,
"xrays": true,
"annihilation": true,
"angular": true,
"angle_optimize": true,
"calc_full_eff": false,
"calc_spectrum": false,
"calc_coincidence": true,
"calc_scattered": true,
"use_exp_transform": false,
"exp_transform_p_parameter": 0.9
```

