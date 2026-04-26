import numpy as np
import skfuzzy as fuzz
from skfuzzy import control as ctrl

# =================================================================
# # Bài 1: Hệ thống giá tiền Grab-Bike
# =================================================================
distance1 = ctrl.Antecedent(np.arange(0, 51, 1), 'distance')
traffic1 = ctrl.Antecedent(np.arange(0, 101, 1), 'traffic')
demand1 = ctrl.Antecedent(np.arange(0, 101, 1), 'demand')
price1 = ctrl.Consequent(np.arange(0, 11, 1), 'price')

distance1.automf(names=['short', 'medium', 'long', 'very_long'])
traffic1.automf(names=['low', 'medium', 'high'])
demand1.automf(names=['low', 'medium', 'high'])
price1.automf(names=['low', 'medium', 'high', 'very_high'])

rules1 = [
    ctrl.Rule(distance1['short'] & traffic1['low'] & demand1['low'], price1['low']),
    ctrl.Rule(distance1['medium'] & traffic1['high'] & demand1['high'], price1['high']),
    ctrl.Rule(distance1['very_long'] & traffic1['high'] & demand1['high'], price1['very_high'])
]

grab_ctrl = ctrl.ControlSystem(rules1)
grab_sim = ctrl.ControlSystemSimulation(grab_ctrl)
grab_sim.input['distance'] = 5
grab_sim.input['traffic'] = 80
grab_sim.input['demand'] = 90
grab_sim.compute()
print(f"Bài 1 - Giá Grab: {grab_sim.output['price']}")


# =================================================================
# # Bài 2: Chiến lược chiết khấu Shopee
# =================================================================
rating2 = ctrl.Antecedent(np.arange(0, 6, 1), 'rating')
sales2 = ctrl.Antecedent(np.arange(0, 101, 1), 'sales')
profit2 = ctrl.Antecedent(np.arange(0, 101, 1), 'profit')
discount2 = ctrl.Consequent(np.arange(0, 71, 1), 'discount')

rating2.automf(names=['low', 'medium', 'high'])
sales2.automf(names=['low', 'medium', 'high'])
profit2.automf(names=['low', 'medium', 'high'])
discount2.automf(names=['very_low', 'low', 'medium', 'high', 'very_high'])

rules2 = [
    ctrl.Rule(rating2['high'] & sales2['high'] & profit['high'], discount2['very_low']),
    ctrl.Rule(rating2['low'] & sales2['low'] & profit['high'], discount2['high']),
    ctrl.Rule(rating2['medium'] & sales2['medium'] & profit['medium'], discount2['medium'])
]

shopee_ctrl = ctrl.ControlSystem(rules2)
shopee_sim = ctrl.ControlSystemSimulation(shopee_ctrl)
shopee_sim.input['rating'] = 3.5
shopee_sim.input['sales'] = 40
shopee_sim.input['profit'] = 80
shopee_sim.compute()
print(f"Bài 2 - Chiết khấu Shopee: {shopee_sim.output['discount']}")


# =================================================================
# # Bài 3: Chiến lược bán hàng Đồng hồ Xa xỉ
# =================================================================
demand3 = ctrl.Antecedent(np.arange(0, 11, 1), 'demand3')
margin3 = ctrl.Antecedent(np.arange(0, 11, 1), 'margin3')
pressure3 = ctrl.Antecedent(np.arange(0, 11, 1), 'pressure3')
strategy3 = ctrl.Consequent(np.arange(0, 101, 1), 'strategy3')

demand3.automf(names=['low', 'medium', 'high'])
margin3.automf(names=['low', 'medium', 'high'])
pressure3.automf(names=['low', 'medium', 'high'])
strategy3.automf(names=['low', 'medium', 'high'])

rules3 = [
    ctrl.Rule(margin3['high'] & demand3['high'] & pressure3['medium'], strategy3['medium'])
]

watch_ctrl = ctrl.ControlSystem(rules3)
watch_sim = ctrl.ControlSystemSimulation(watch_ctrl)
watch_sim.input['demand3'] = 9
watch_sim.input['margin3'] = 9
watch_sim.input['pressure3'] = 5
watch_sim.compute()
print(f"Bài 3 - Giảm giá đồng hồ: {watch_sim.output['strategy3']}")


# =================================================================
# # Bài 4: Tối ưu hóa giao hàng (Logistics)
# =================================================================
density4 = ctrl.Antecedent(np.arange(0, 11, 1), 'density4')
load4 = ctrl.Antecedent(np.arange(0, 11, 1), 'load4')
combine4 = ctrl.Consequent(np.arange(0, 11, 1), 'combine4')

density4.automf(names=['low', 'medium', 'high'])
load4.automf(names=['low', 'medium', 'high'])
combine4.automf(names=['few', 'some', 'many'])

rules4 = [
    ctrl.Rule(density4['high'] & load4['low'], combine4['many']),
    ctrl.Rule(density4['low'] & load4['high'], combine4['few'])
]

logi_ctrl = ctrl.ControlSystem(rules4)
logi_sim = ctrl.ControlSystemSimulation(logi_ctrl)
logi_sim.input['density4'] = 9
logi_sim.input['load4'] = 2
logi_sim.compute()
print(f"Bài 4 - Số đơn kết hợp: {logi_sim.output['combine4']}")
