# outline

--------

@File    :   outline.md
@Time    :   2022/05/15 18:43:25
@Auth    :   Ming(<3057761608@qq.com>)
@Vers    :   0.1
@Desc    :   outline of evl_word
@Refe    :   url1; url2

--------

## env

### 初始

- 三维空间：二维+高度
- 属性
  - 系统
    - 边界 x_min=0 y_min=0 h_min=0 x/y/h_max 
    - 系统不稳定值 sys_ubstab_value ：生成地图时使用
  - 空间块
    - 位置 location：三维坐标 (x,y,h)
    - 物理数值 physical_value：0，1表示空气和实地
- 划分
  - 依据 physical_value 划分空气和实地
- 生成规则：输入世界种子word_seed
  - 初始生成
    - 输入 word_seed + localtion，生成当前位置的physical_value
    - 生成函数
  - 重力作用：置换
    - 定义 压力值 press_value=sum(this| x=this_x, y=this_y, h>this_h)
    - 定义 系统不稳定值 sys_ubstab_value=sum(this | press_value*(h_max-this_h)) 
    - 自下而上遍历，当 physical_value=0 且 press_value>0 时，当前空间块与其上一个空间块的 physical_value 互换
    - 重复上述过程，直至 sys_ubstab_value=0
- 导出地表 surface

### 后期

- 三维空间
  - 边界：周期边界条件
  - 属性
    - 位置location：三维坐标
    - 营养丰度nutri：提供给plant
    - 稳定程度stab：是否易破坏，生成地形；提供给animal
  - 划分
    - 土壤：nutri 高；stab 低
    - 岩石：nutri 低；stab 高
    - 沙砾：nutri 低；stab 低
    - 空气：nutri 极低； stab 极低
  - 生成规则：输入世界种子word_seed
    - 初始生成
      - 输入word_seed + localtion，随机生成当前位置的 (nutri, stab)
      - stab 高时， nutri 低
      - stab 极低时， nutri 低
    - 重力作用，置换？
    - 生成水源
      - 低于水平面，且表面
  - 演化规则

## plant

### 初始

- 覆盖于地表 surface
- 属性
  - 位置location：三维坐标
  - 物理数值 physical_value：0，1表示无和有
- 划分
  - 依据 physical_value：0，1 分无和有
- 生成规则
  - 初始生成
    - 输入 word_seed + localtion，生成当前位置的physical_value

### 后期

- 依附于环境，不同环境生长难易不同
  - 土壤，易生长
  - 岩石、沙地，难生长

## herbivores

### 初始

### 后期

## carnivore

### 初始

### 后期