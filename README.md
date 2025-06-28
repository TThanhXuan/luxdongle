# lux dongle.

## Phương án thay thế cho dongle lux
- Đưa dữ liệu update 1-2s lên homeassistant.
- Đưa dữ liệu lên server lux, nhận cài đặt từ server lux như dongle hãng.
## Hướng dẫn sử dụng

### 1. Các đèn báo trên dongle
- Đèn 1: trạng thái kết nối wifi
- Đèn 2: trạng thái kết nối inverter
- Đèn 3: trạng thái kết nối cloud của solar box (bạn nào không đăng ký cloud có thể bỏ qua đèn này)
- Đèn 4: trạng thái kết nối cloud lux power nếu bạn có bật chức năng đẩy data về lux server.

### 2. Kết nối wifi cho dongle (chỉ làm 1 lần)

- Cấp nguồn dongle và chờ khoản 1 phút.
- Kết nối đến wifi: solarbox_dongle
- Vào địa chỉ http://192.168.4.1 để cài đặt wifi

### 3. Đăng nhập vào solarbox cloud (chỉ cho ai có sử dụng cloud)

- Đăng nhập với thông tin mình gửi. [Video hướng dẫn](https://youtube.com/shorts/DpIyl63lWtc?feature=share)

### 4. Kết nối dongle với homeassistant trong mạng local 

- Khi thiết bị kết nối với wifi trên homeassistant sẽ hiện thông báo nhận thiết bị mới. các bạn nhấn thêm thiết bị.

- Cài đặt hiển thị unsynk-power-flow-card:

```yaml
type: custom:stack-in-card
cards:
  - type: custom:sunsynk-power-flow-card
    cardstyle: full
    show_solar: true
    inverter:
      model: lux
      modern: false
      autarky: "no"
      colour:
        - 100
        - 196
        - 102
      three_phase: false
      auto_scale: true
    battery:
      show: true
      shutdown_soc: 20
      show_daily: true
      invert_power: false
      dynamic_colour: true
      animate: true
      show_remaining_energy: true
      show_absolute: false
      auto_scale: true
      count: 1
      animation_speed: 1
      invert_flow: true
      remaining_energy_to_shutdown: false
      hide_soc: false
      linear_gradient: true
      energy: number.dg_005034_pin
    solar:
      show_daily: true
      mppts: 2
      pv1_name: PV 1
      pv2_name: PV 2
      dynamic_colour: true
      animation_speed: 1
      display_mode: 3
      pv2_max_power: number.dg_005034_pv2
      max_power: number.dg_005034_pv
      pv1_max_power: number.dg_005034_pv1
    load:
      show_daily: true
      essential_name: TẢI NHÀ
      animation_speed: 1
    grid:
      show_daily_buy: true
      show_daily_sell: true
      show_nonessential: false
      invert_grid: false
      additional_loads: 2
      no_grid_colour:
        - 117
        - 122
        - 121
      grid_off_colour:
        - 255
        - 0
        - 0
      colour:
        - 100
        - 196
        - 102
      animation_speed: 1
      auto_scale: true
    entities:
      inverter_voltage_154: sensor.dg_005034_ivt_grid_voltage
      load_frequency_192: sensor.dg_005034_ivt_grid_frequency
      day_battery_charge_70: sensor.dg_005034_ivt_battery_charge_today
      day_battery_discharge_71: sensor.dg_005034_ivt_battery_discharge_today
      battery_voltage_183: sensor.dg_005034_ivt_battery_voltage
      battery_soc_184: sensor.dg_005034_ivt_battery_soc
      battery_power_190: sensor.dg_005034_ivt_battery_power
      battery_current_191: sensor.dg_005034_ivt_battery_current
      day_grid_import_76: sensor.dg_005034_ivt_grid_import_today
      day_grid_export_77: sensor.dg_005034_ivt_grid_export_today
      grid_ct_power_172: sensor.dg_005034_ivt_grid_power
      essential_power: sensor.dg_005034_ivt_load_power
      nonessential_power: none
      aux_power_166: sensor.aux_output_power
      day_pv_energy_108: sensor.dg_005034_ivt_pv_today
      pv1_power_186: sensor.dg_005034_ivt_pv1_power
      pv2_power_187: sensor.dg_005034_ivt_pv2_power
      pv1_voltage_109: sensor.dg_005034_ivt_pv1_voltage
      pv2_voltage_111: sensor.dg_005034_ivt_pv2_voltage
      pv2_current_112: sensor.dg_005034_ivt_pv2_current
      radiator_temp_91: sensor.dg_005034_ivt_ac_temp
      dc_transformer_temp_90: sensor.dg_005034_ivt_dc_temp
      pv1_current_110: sensor.dg_005034_ivt_pv1_current
      day_load_energy_84: sensor.dg_005034_ivt_load_today
      grid_power_169: sensor.dg_005034_ivt_grid_power
      inverter_current_164: sensor.dg_005034_ivt_inverter_rms_current
      inverter_power_175: sensor.dg_005034_ivt_inverter_power
      total_pv_generation: sensor.dg_005034_ivt_pv_total
      pv_total: sensor.dg_005034_ivt_pv_power
    show_battery: true
    panel_mode: true
    large_font: true
    show_grid: true
    dynamic_line_width: true
    min_line_width: 4
  - type: markdown
    content: >-
      <center>Điều chỉnh giới hạn dòng xả theo thời gian đang
      được:&nbsp;<font>{{states('switch.dg_005034_tu_dong_gh_dong_xa') | upper
      }} </font> <center>Giới hạn dòng xả hiện
      tại:&nbsp;<font>{{states('number.dg_005034_ivt_discharge_current')}}A
      </font>
    text_only: true
```

- Hiển thị thống kê:

```yaml
type: vertical-stack
cards:
  - type: horizontal-stack
    cards:
      - type: custom:mushroom-entity-card
        entity: sensor.dg_005034_ivt_load_today
        name: Tiêu thụ
        icon: ""
        icon_color: pink
      - type: custom:mushroom-entity-card
        entity: sensor.dg_005034_ivt_pv_today
        name: Sản lượng
        icon: mdi:solar-power
        icon_color: primary
  - type: horizontal-stack
    cards:
      - type: custom:mushroom-entity-card
        entity: sensor.dg_005034_ivt_grid_import_today
        name: Lấy lưới
        icon: mdi:transmission-tower-export
        icon_color: indigo
      - type: custom:mushroom-entity-card
        entity: sensor.dg_005034_ivt_grid_export_today
        name: Đẩy lưới
        icon: mdi:transmission-tower-import
        icon_color: amber
  - type: horizontal-stack
    cards:
      - type: custom:mushroom-entity-card
        entity: sensor.dg_005034_ivt_battery_charge_today
        name: Sạc Pin
        icon: mdi:battery-plus-variant
        icon_color: green
      - type: custom:mushroom-entity-card
        entity: sensor.dg_005034_ivt_battery_discharge_today
        name: Xả Pin
        icon: mdi:battery-minus-variant
        icon_color: deep-orange
  - type: horizontal-stack
    cards:
      - type: custom:mushroom-entity-card
        entity: sensor.dg_005034_ivt_pv1_total
        name: Tổng PV1
        icon: mdi:solar-panel
      - type: custom:mushroom-entity-card
        entity: sensor.dg_005034_ivt_pv2_total
        name: Tổng PV2
        icon: mdi:solar-panel
  - type: horizontal-stack
    cards:
      - type: custom:mushroom-entity-card
        entity: sensor.dg_005034_ivt_pv_total
        name: Tổng PV
        icon: mdi:solar-panel
      - type: custom:mushroom-entity-card
        entity: sensor.dg_005034_ivt_load_total
        name: Tổng Tải
  - type: horizontal-stack
    cards:
      - type: custom:mushroom-entity-card
        entity: sensor.dg_005034_ivt_grid_import_total
        name: Tổng Lấy Lưới
        icon: mdi:transmission-tower-export
      - type: custom:mushroom-entity-card
        entity: sensor.dg_005034_ivt_grid_export_total
        name: Tổng Đẩy Lưới
        icon: mdi:transmission-tower-import
```

- Hiển thị vòng horseshoe:
```yaml
type: horizontal-stack
cards:
  - type: custom:flex-horseshoe-card
    entities:
      - id: 0
        entity: sensor.dg_005034_bms1_state_of_charge
        decimals: 0
        name: Pin
      - id: 1
        entity: sensor.dg_005034_ivt_battery_discharge_today
        decimals: 1
        name: Xả hôm nay
        unit: kW
        icon: mdi:battery-minus
      - id: 2
        entity: sensor.dg_005034_ivt_battery_charge_today
        decimals: 1
        name: Sạc hôm nay
        unit: kW
        icon: mdi:battery-plus
      - id: 3
        entity: sensor.dg_005034_ivt_battery_power
        decimals: 0
        name: PIN Lithium
        icon: mdi:home-battery-outline
    show:
      scale_tickmarks: false
      horseshoe_style: colorstop
    layout:
      hlines:
        - id: 0
          xpos: 50
          ypos: 40
          length: 60
          styles:
            - stroke: var(--primary-text-color);
            - stroke-width: 3;
            - stroke-linecap: round;
            - opacity: 0.7;
        - id: 1
          xpos: 50
          ypos: 60
          length: 60
          styles:
            - stroke: var(--primary-text-color);
            - stroke-width: 3;
            - stroke-linecap: round;
            - opacity: 0.7;
      vlines:
        - id: 0
          xpos: 50
          ypos: 50
          length: 20
          styles:
            - stroke: var(--primary-text-color);
            - stroke-width: 3;
            - stroke-linecap: round;
            - opacity: 0.7;
      states:
        - id: 0
          entity_index: 0
          xpos: 55
          ypos: 32
          styles:
            - font-size: 2.5em;
        - id: 1
          entity_index: 1
          xpos: 40
          ypos: 54
          styles:
            - font-size: 1.5em;
        - id: 2
          entity_index: 2
          xpos: 80
          ypos: 54
          styles:
            - font-size: 1.5em;
        - id: 3
          entity_index: 3
          xpos: 57
          ypos: 77
          styles:
            - font-size: 2.0em;
      names:
        - id: 0
          entity_index: 0
          xpos: 50
          ypos: 95
          styles:
            - font-size: 1.5em;
      icons:
        - id: 0
          entity_index: 0
          xpos: 27
          ypos: 30
          size: 2.5
        - id: 1
          entity_index: 1
          xpos: 20
          ypos: 53
          size: 2.5
        - id: 2
          entity_index: 2
          xpos: 60
          ypos: 53
          size: 2.5
        - id: 3
          entity_index: 3
          xpos: 30
          ypos: 75
          size: 2.5
    horseshoe_scale:
      min: 0
      max: 100
      width: 10
    color_stops:
      "0": "#F70202"
      "20": "#FFDE59"
      "50": "#18F80D"
  - type: custom:flex-horseshoe-card
    entities:
      - id: 0
        entity: sensor.dg_005034_ivt_pv_power
        decimals: 0
        name: PV
        unit: ""
        icon: mdi:solar-power
      - id: 1
        entity: sensor.dg_005034_ivt_pv1_today
        decimals: 1
        name: PV1
        unit: kW
        icon: mdi:solar-power-variant
      - id: 2
        entity: sensor.dg_005034_ivt_pv2_today
        decimals: 1
        name: PV2
        unit: kW
        icon: mdi:solar-power-variant
      - id: 3
        entity: sensor.dg_005034_ivt_pv_today
        decimals: 1
        name: PV Today
        icon: mdi:solar-power-variant
    show:
      scale_tickmarks: false
      horseshoe_style: colorstop
    layout:
      hlines:
        - id: 0
          xpos: 50
          ypos: 40
          length: 60
          styles:
            - stroke: var(--primary-text-color);
            - stroke-width: 3;
            - stroke-linecap: round;
            - opacity: 0.7;
        - id: 1
          xpos: 50
          ypos: 60
          length: 60
          styles:
            - stroke: var(--primary-text-color);
            - stroke-width: 3;
            - stroke-linecap: round;
            - opacity: 0.7;
      vlines:
        - id: 0
          xpos: 50
          ypos: 50
          length: 20
          styles:
            - stroke: var(--primary-text-color);
            - stroke-width: 3;
            - stroke-linecap: round;
            - opacity: 0.7;
      states:
        - id: 0
          entity_index: 0
          xpos: 55
          ypos: 32
          styles:
            - font-size: 2.5em;
        - id: 1
          entity_index: 1
          xpos: 40
          ypos: 54
          styles:
            - font-size: 1.5em;
        - id: 2
          entity_index: 2
          xpos: 80
          ypos: 54
          styles:
            - font-size: 1.5em;
        - id: 3
          entity_index: 3
          xpos: 57
          ypos: 77
          styles:
            - font-size: 2.0em;
      names:
        - id: 0
          entity_index: 0
          xpos: 50
          ypos: 95
          styles:
            - font-size: 1.5em;
      icons:
        - id: 0
          entity_index: 0
          xpos: 27
          ypos: 30
          size: 2.5
        - id: 1
          entity_index: 1
          xpos: 20
          ypos: 53
          size: 2.5
        - id: 2
          entity_index: 2
          xpos: 60
          ypos: 53
          size: 2.5
        - id: 3
          entity_index: 3
          xpos: 30
          ypos: 75
          size: 2.5
    horseshoe_scale:
      min: 0
      max: 6000
      width: 7
    color_stops:
      "0": "#60E92A"
      "4000": "#FFDE59"
      "5000": "#F04040"
```
- Hiển thị cài đặt công suất:
```yaml
type: vertical-stack
cards:
  - type: horizontal-stack
    cards:
      - type: custom:mushroom-entity-card
        entity: number.dg_005034_pv1
        name: CS lắp đặt PV1
        icon: mdi:solar-panel
      - type: custom:mushroom-entity-card
        entity: number.dg_005034_pv2
        name: CS lắp đặt PV2
        icon: mdi:solar-panel
  - type: horizontal-stack
    cards:
      - type: custom:mushroom-entity-card
        entity: number.dg_005034_pv
        name: CS lắp đặt PV
        icon: mdi:solar-panel-large
      - type: custom:mushroom-entity-card
        entity: number.dg_005034_pin
        name: NL Pin Lưu Trữ
        icon: mdi:battery
title: Cài đặt hiển thị
```
- Hiển thị đẩy dữ liệu đến server lux

```yaml
type: vertical-stack
cards:
  - type: horizontal-stack
    cards:
      - type: custom:mushroom-entity-card
        entity: switch.dg_005034_gui_data_den_lux_server
        secondary_info: state
        primary_info: name
        name: Trạng Thái
        tap_action:
          action: toggle
        icon_type: icon
        icon_color: accent
  - type: vertical-stack
    cards:
      - type: custom:mushroom-entity-card
        entity: text.dg_005034_dongle_sn
        name: Dongle serial, trên dongle hãng. !! Cần cài đặt !!
      - type: custom:mushroom-entity-card
        entity: sensor.dg_005034_dongle_serial_number
        name: Dongle serial, đã được cài đặt
      - type: custom:mushroom-entity-card
        entity: sensor.dg_005034_ivt_serial_number
        name: Inverter serial number
title: Cài đặt gửi data đến server lux
```


- Hiển thị cài đặt giới hạn dòng xả theo giờ:
```yaml
type: vertical-stack
cards:
  - type: horizontal-stack
    cards:
      - type: custom:mushroom-entity-card
        entity: switch.dg_005034_tu_dong_gh_dong_xa
        secondary_info: state
        primary_info: name
        name: Trạng Thái
        tap_action:
          action: toggle
        icon_type: icon
        icon_color: accent
  - type: horizontal-stack
    cards:
      - type: custom:mushroom-entity-card
        entity: number.dg_005034_gh_dong_xa_0h_6h
        name: Giới hạn 0h-6h
      - type: custom:mushroom-entity-card
        entity: number.dg_005034_gh_dong_xa_6h_12h
        name: Giới hạn 6h-12h
  - type: horizontal-stack
    cards:
      - type: custom:mushroom-entity-card
        entity: number.dg_005034_gh_dong_xa_12h_18h
        name: Giới hạn 12h-18h
      - type: custom:mushroom-entity-card
        entity: number.dg_005034_gh_dong_xa_18h_24h
        name: Giới hạn 18h-24h
title: Cài đặt dòng xả theo thời gian
```

### 5. Thay đổi một số thông tin của hệ thống của bạn
- CS lắp đặt PV: là công suất lắp đặt tổng PV của bạn (dv: W)
- CS lắp đặt PV1: là công suất lắp đặt tổng PV1 của bạn (dv: W)
- CS lắp đặt PV2: là công suất lắp đặt tổng PV2 của bạn (dv: W)
- NL Pin Lưu trữ: khả năng lưu trữ của pin/ acquy (dv Wh)

### 6. Cài đặt giới hạn dòng xả theo giờ.
- Cài đặt giới hạn dòng xả theo khung giờ.
- Bật switch "trạng thái" nếu bạn muốn sử dụng chức năng này

### 7. Gửi data đến server lux như dongle hãng
- Điền Serial number của dongle của bạn (vd: BAXXXXXXXX)
- Bật chức năng gửi data đến server lux

