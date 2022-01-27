# Спецификация in-файлов

**версия 1.0**

## 1. Общее описание формата

Файлы типа `*.*in` хранят описание моделей детекторов (`*.din`), источников (`*.sin`), анализаторов (`*.ain`)

Параметры модели записываются в виде строчек: ключ -- значение, разделённые символом `=`: `key = value`, строчки могут идти в любом порядке (но для удобочитаемости рекомендуется их группировать по секциям), количество пробелов слева и справа от символа `=` не важно.

В файле могут быть комментарии, комментарии начинаются с `#` или `//` в начале строке.

В файле могут быть пустые строки -- они игнорируются.

В файле могут быть дополнительные (информационные) строки в формате ключ=значения, которые не поддерживаются расчётными модулями, они будут игнорироваться.

Дублирующиеся строчки в файлы запрещены и приведут к неопределённому поведению (будет считана какая-то одна из них).



## 2. Описание модели детектора

2.1 Должен быть указан тип детектора:

`DetectorType = DETECTOR_TYPE`, где `DETECTOR_TYPE` может принимать значения: `COAXIAL`, `SCINTILLATOR`, `COAX_WELL`.

2.2 Должны быть указаны размеры параметров детектора в формате: `Dimension = 12.3 cm`, размеры могут быть указаны с ` cm`, так и без указания единиц.

Список возможных параметров детекторов приведён в разделе 5.

2.3 Должны быть указаны материалы детектора в формате:
```ini
{DETECTOR_PREFIX}_n{LAYER_NAME}Elements = int_value
{DETECTOR_PREFIX}_Ro{LAYER_NAME} = double_value
{DETECTOR_PREFIX}_Z{LAYER_NAME}[{ELEMENT_INDEX}] = int_value
{DETECTOR_PREFIX}_Fractions{LAYER_NAME}[{ELEMENT_INDEX}] = double_value
{DETECTOR_PREFIX}_FractionType{LAYER_NAME} = MASS or STOCHIOMETRIC
... for all ELEMENT_INDEX
```

`{DETECTOR_PREFIX}` может принимать значения: `DC` для `COAXIAL`, `DS` для `SCINTILLATOR`, `DCW` для `COAX_WELL`

`LAYER_NAME` -- имя слоя детектора (список см. ниже)

`ELEMENT_INDEX` -- индекс элемента (число от 0 до числа элементов в материале слоя детектора)

Поле `{DETECTOR_PREFIX}_FractionType{LAYER_NAME}` необязательное и имеет значение по умолчанию: `MASS`

Вместо поля `{DETECTOR_PREFIX}_n{LAYER_NAME}Elements` можно указывать `{DETECTOR_PREFIX}_n{LAYER_NAME}`, хотя и не рекомендуется.



## 3. Описание модели источника

3.1 Должен быть указан тип источника:

`SourceType = SOURCE_TYPE`, где `SOURCE_TYPE` может принимать: `POINT`, `CYLINDER`, `MARINELLI`, `CONE`.

3.2 Форматы секций размеров и материалов источника идентичны форматам детектора.

3.3 Список возможных параметров источников приведён в разделе 6.



## 4. Описание модели анализатора

Модель анализатора имеет фиксированный набор полей:

```ini
AN_FWHM_122 = 0.77
AN_FWHM_662 = 1.26 
AN_FWHM_1332 = 1.73
AN_kev_per_ch = 0.1831
AN_N_ch = 16384
```

`AN_FWHM_122`, `AN_FWHM_662`, `AN_FWHM_1332` -- значения FWHM для энергий 122, 662 и 1332 кэВ, должно быть указано минимум одно из полей.

`AN_kev_per_ch` -- цена канала (обязательное поле)

`AN_N_ch` -- число каналов (обязательное поле)



## 5. Список возможных полей для детекторов

### 5.1 Коаксиальный детектор (coaxial)

Пример задания размеров детектора типа `COAXIAL` (все поля обязательны):

```ini
DC_CrystalDiameter = 5.41 cm
DC_CrystalHeight = 3.99 cm
DC_CrystalHoleDiameter = 0.85 cm
DC_CrystalHoleHeight = 2.6 cm
DC_CrystalFrontDeadLayer = 0.07 cm
DC_CrystalSideDeadLayer = 0.07 cm
DC_CrystalBackDeadLayer = 0.07 cm
DC_CrystalHoleBottomDeadLayer = 0.0003 cm
DC_CrystalHoleSideDeadLayer = 0.0003 cm
DC_CrystalSideCladdingThickness = 0.08 cm
DC_CapToCrystalDistance = 0.3 cm
DC_DetectorCapDiameter = 7 cm
DC_DetectorCapFrontThickness = 0.1 cm
DC_DetectorCapSideThickness = 0.1 cm
DC_DetectorCapBackThickness = 0.1 cm
DC_DetectorMountingThickness = 3 cm
```

Пример задания материалов детектора типа `COAXIAL` (поля, кроме `FractionType`, являются обязательными):

```ini
# Crystal
DC_nCrystalElements = 1
DC_RoCrystal = 5.323
DC_ZCrystal[0] = 32
DC_FractionsCrystal[0] = 1
DC_FractionTypeCrystal = MASS

# Crystal Cladding
DC_nCrystalSideCladdingElements = 1
DC_RoCrystalSideCladding = 2.7
DC_ZCrystalSideCladding[0] = 13
DC_FractionsCrystalSideCladding[0] = 1
DC_FractionTypeCrystalSideCladding = MASS

# Crystal Mounting
DC_nCrystalMountingElements = 1
DC_RoCrystalMounting = 2.7
DC_ZCrystalMounting[0] = 13
DC_FractionsCrystalMounting[0] = 1
DC_FractionTypeCrystalMounting = MASS

# Detector Cap
DC_nDetectorCapElements = 1
DC_RoDetectorCap = 2.7
DC_ZDetectorCap[0] = 13
DC_FractionsDetectorCap[0] = 1
DC_FractionTypeDetectorCap = MASS

# Vacuum
DC_nVacuumElements = 1
DC_RoVacuum = 1E-10
DC_ZVacuum[0] = 13
DC_FractionsVacuum[0] = 1
DC_FractionTypeVacuum = MASS
```

### 5.2 Детектор типа сцинтиллятора (scintillator)

Пример задания размеров детектора типа `SCINTILLATOR` (все поля обязательны):

```ini
DS_CrystalDiameter = 4 cm
DS_CrystalHeight = 4 cm
DS_CrystalFrontReflectorThickness = 0.1 cm
DS_CrystalSideReflectorThickness = 0.1 cm
DS_CrystalFrontCladdingThickness = 0.1 cm
DS_CrystalSideCladdingThickness = 0.1 cm
DS_DetectorFrontPackagingThickness = 0.2 cm
DS_DetectorSidePackagingThickness = 0.5 cm
DS_DetectorFrontCapThickness = 0.1 cm
DS_DetectorSideCapThickness = 0.1 cm
DS_DetectorMountingThickness = 3 cm
```

Пример задания материалов детектора типа `SCINTILLATOR` (поля, кроме `FractionType`, являются обязательными):

```ini
# Crystal
DS_nCrystalElements = 2
DS_RoCrystal = 3.667
DS_ZCrystal[0] = 11
DS_FractionsCrystal[0] = 0.153373
DS_ZCrystal[1] = 53
DS_FractionsCrystal[1] = 0.846627
DS_FractionTypeCrystal = MASS

# Crystal Cladding
DS_nCrystalCladdingElements = 1
DS_RoCrystalCladding = 2.7
DS_ZCrystalCladding[0] = 13
DS_FractionsCrystalCladding[0] = 1
DS_FractionTypeCrystalCladding = MASS

# Reflector
DS_nCrystalReflectorElements = 2
DS_RoCrystalReflector = 3.58
DS_ZCrystalReflector[0] = 8
DS_FractionsCrystalReflector[0] = 0.396964
DS_ZCrystalReflector[1] = 12
DS_FractionsCrystalReflector[1] = 0.603036
DS_FractionTypeCrystalReflector = MASS

# DetectorPackaging
DS_nDetectorPackagingElements = 2
DS_RoDetectorPackaging = 0.04
DS_ZDetectorPackaging[0] = 1
DS_FractionsDetectorPackaging[0] = 0.078
DS_ZDetectorPackaging[1] = 6
DS_FractionsDetectorPackaging[1] = 0.922
DS_FractionTypeDetectorPackaging = MASS

# DetectorCap
DS_nDetectorCapElements = 1
DS_RoDetectorCap = 2.7
DS_ZDetectorCap[0] = 13
DS_FractionsDetectorCap[0] = 1
DS_FractionTypeDetectorCap = MASS
```

### 5.3 Колодезный детектор (well detector)

Пример задания размеров детектора типа `COAX_WELL` (все поля обязательны):

```ini
DCW_CrystalDiameter = 5.75 cm
DCW_CrystalHeight = 5.53 cm
DCW_CrystalHoleDiameter = 2.97 cm
DCW_CrystalHoleHeight = 3.6 cm
DCW_CrystalFrontDeadLayer = 0.07 cm
DCW_CrystalSideDeadLayer = 0.07 cm
DCW_CrystalBackDeadLayer = 0.07 cm
DCW_CrystalHoleBottomDeadLayer = 0.0003 cm
DCW_CrystalHoleSideDeadLayer = 0.0003 cm
DCW_CrystalSideCladdingThickness = 0.1 cm
DCW_CapToCrystalDistance = 0.9 cm
DCW_DetectorCapDiameter = 9.3 cm
DCW_DetectorCapFrontThickness = 0.2 cm
DCW_DetectorCapSideThickness = 0.15 cm
DCW_DetectorCapBackThickness = 0.15 cm
DCW_DetectorCapHoleDiameter = 1.6 cm
DCW_DetectorCapHoleHeight = 4 cm
DCW_DetectorCapHoleBottomThickness = 0.07 cm
DCW_DetectorCapHoleSideThickness = 0.07 cm
DCW_DetectorMountingThickness = 3 cm
```

Пример задания материалов детектора типа `COAX_WELL` (поля, кроме `FractionType`, являются обязательными):

```ini
# Crystal
DCW_nCrystalElements = 1
DCW_RoCrystal = 5.323
DCW_ZCrystal[0] = 32
DCW_FractionsCrystal[0] = 1
DCW_FractionTypeCrystal = MASS

# Crystal Cladding
DCW_nCrystalSideCladdingElements = 1
DCW_RoCrystalSideCladding = 2.7
DCW_ZCrystalSideCladding[0] = 13
DCW_FractionsCrystalSideCladding[0] = 1
DCW_FractionTypeCrystalSideCladding = MASS

# Crystal Mounting
DCW_nCrystalMountingElements = 1
DCW_RoCrystalMounting = 2.7
DCW_ZCrystalMounting[0] = 13
DCW_FractionsCrystalMounting[0] = 1
DCW_FractionTypeCrystalMounting = MASS

# Detector Cap
DCW_nDetectorCapElements = 1
DCW_RoDetectorCap = 2.7
DCW_ZDetectorCap[0] = 13
DCW_FractionsDetectorCap[0] = 1
DCW_FractionTypeDetectorCap = MASS

# Vacuum
DCW_nVacuum = 1
DCW_RoVacuum = 1E-10
DCW_ZVacuum[0] = 13
DCW_FractionsVacuum[0] = 1
DCW_FractionTypeVacuum = MASS
```



## 6. Список возможных полей источников

### 6.1 Точечный (point) источник

Пример задания размеров источника типа `POINT`:

```ini
pdistance = 10.0 cm
prho = 0.0 cm
```

Поле `pdistance` -- обязательно, поле `prho` имеет дефолтное значение -- 0.

Секции материалов нет.

### 6.2 Цилиндрический (cylinder) источник

Пример задания размеров источника типа `CYLINDER` (все поля обязательны):

```ini
SC_BeakerToDetectorFrontDistance = 0 cm
SC_BeakerDiameter = 8.8 cm
SC_BeakerHeight = 1.5 cm
SC_BeakerSideWallThickness = 0.1 cm
SC_BeakerEndWallThickness = 0.1 cm
SC_SourceHeight = 1.3 cm
```

Пример задания материалов источника типа `CYLINDER` (поля, кроме `FractionType`, являются обязательными):

```ini
# Walls
SC_nWallElements = 3
SC_RoWall = 1.38
SC_ZWall[0] = 1
SC_FractionsWall[0] = 0.04196
SC_ZWall[1] = 6
SC_FractionsWall[1] = 0.625016
SC_ZWall[2] = 8
SC_FractionsWall[2] = 0.333024
SC_FractionTypeWall = MASS

# Source
SC_nSourceElements = 2
SC_RoSource = 1
SC_ZSource[0] = 1
SC_FractionsSource[0] = 0.111899330013933
SC_ZSource[1] = 8
SC_FractionsSource[1] = 0.888100669986067
SC_FractionTypeSource = MASS

# Empty Space
SC_nEmptySpaceElements = 3
SC_RoEmptySpace = 0.001
SC_ZEmptySpace[0] = 7
SC_FractionsEmptySpace[0] = 0.755
SC_ZEmptySpace[1] = 8
SC_FractionsEmptySpace[1] = 0.2315
SC_ZEmptySpace[2] = 18
SC_FractionsEmptySpace[2] = 0.0129
SC_FractionTypeEmptySpace = MASS
```

### 6.3 Конический источник (cone)

Пример задания размеров источника типа `CONE` (все поля обязательны):

```ini
SK_BeakerToDetectorFrontDistance = 0 cm
SK_BeakerDiameterBottom = 6.7 cm
SK_BeakerDiameterTop = 7.2 cm
SK_BeakerHeight = 3.6 cm
SK_BeakerSideWallThickness = 0.1 cm
SK_BeakerEndWallThickness = 0.1 cm
SK_SourceHeight = 3.4 cm
```

Пример задания материалов источника типа `CONE` (поля, кроме `FractionType`, являются обязательными):

```ini
# Walls
SK_nWallElements = 3
SK_RoWall = 1.38
SK_ZWall[0] = 1
SK_FractionsWall[0] = 0.04196
SK_ZWall[1] = 6
SK_FractionsWall[1] = 0.625016
SK_ZWall[2] = 8
SK_FractionsWall[2] = 0.333024
SK_FractionTypeWall = MASS

# Source
SK_nSourceElements = 2
SK_RoSource = 1
SK_ZSource[0] = 1
SK_FractionsSource[0] = 0.111899330013933
SK_ZSource[1] = 8
SK_FractionsSource[1] = 0.888100669986067
SK_FractionTypeSource = MASS

# Empty Space
SK_nEmptySpaceElements = 3
SK_RoEmptySpace = 0.001
SK_ZEmptySpace[0] = 7
SK_FractionsEmptySpace[0] = 0.755
SK_ZEmptySpace[1] = 8
SK_FractionsEmptySpace[1] = 0.2315
SK_ZEmptySpace[2] = 18
SK_FractionsEmptySpace[2] = 0.0129
SK_FractionTypeEmptySpace = MASS
```

### 6.4 Источник типа Маринелли (marinelli)

Пример задания размеров источника типа `MARINELLI` (все поля обязательны):

```ini
SM_BeakerToDetectorFrontDistance = 0 cm
SM_BeakerDiameter = 15.4 cm
SM_BeakerHeight = 11.2 cm
SM_BeakerHoleDiameter = 9.6 cm
SM_BeakerHoleHeight = 6.6 cm
SM_BeakerSideThickness = 0.2 cm
SM_BeakerEndWallThickness = 0.2 cm
SM_BeakerHoleSideThickness = 0.2 cm
SM_BeakerHoleEndWallThickness = 0.2 cm
SM_SourceHeight = 8.6 cm
```

Пример задания материалов источника типа `MARINELLI` (поля, кроме `FractionType`, являются обязательными):

```ini
# Walls
SM_nWallElements = 3
SM_RoWall = 1.38
SM_ZWall[0] = 1
SM_FractionsWall[0] = 0.04196
SM_ZWall[1] = 6
SM_FractionsWall[1] = 0.625016
SM_ZWall[2] = 8
SM_FractionsWall[2] = 0.333024
SM_FractionTypeWall = MASS

# Source
SM_nSourceElements = 2
SM_RoSource = 1
SM_ZSource[0] = 1
SM_FractionsSource[0] = 0.111899330013933
SM_ZSource[1] = 8
SM_FractionsSource[1] = 0.888100669986067
SM_FractionTypeSource = MASS

# Empty Space
SM_nEmptySpaceElements = 3
SM_RoEmptySpace = 0.001
SM_ZEmptySpace[0] = 7
SM_FractionsEmptySpace[0] = 0.755
SM_ZEmptySpace[1] = 8
SM_FractionsEmptySpace[1] = 0.2315
SM_ZEmptySpace[2] = 18
SM_FractionsEmptySpace[2] = 0.0129
SM_FractionTypeEmptySpace = MASS
```

