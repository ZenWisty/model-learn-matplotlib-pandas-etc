

```python
from __future__ import (absolute_import, division, print_function,
                        unicode_literals)
try:
    # Python 2.x 版本
    from urllib2 import urlopen
except ImportError:
    # Python 3.x 版本
    from urllib.request import urlopen  # 1
```


```python
import json
import pygal
import math
from itertools import groupby
```


```python
json_url = 'https://raw.githubusercontent.com/muxuezi/btc/master/btc_close_2017.json'
# 第一种方式：使用 urlopen 和 .read() 获得
response = urlopen(json_url)
req = response.read()
with open('btc_close_2017_urllib.json','wb') as f:
    f.write(req)
file_urllib = json.loads(req.decode('utf8'))
print(file_urllib)
```

    [{'date': '2017-01-01', 'month': '01', 'week': '52', 'weekday': 'Sunday', 'close': '6928.6492'}, {'date': '2017-01-02', 'month': '01', 'week': '1', 'weekday': 'Monday', 'close': '7070.2554'}, {'date': '2017-01-03', 'month': '01', 'week': '1', 'weekday': 'Tuesday', 'close': '7175.1082'}, {'date': '2017-01-04', 'month': '01', 'week': '1', 'weekday': 'Wednesday', 'close': '7835.7615'}, {'date': '2017-01-05', 'month': '01', 'week': '1', 'weekday': 'Thursday', 'close': '6928.7578'}, {'date': '2017-01-06', 'month': '01', 'week': '1', 'weekday': 'Friday', 'close': '6196.6928'}, {'date': '2017-01-07', 'month': '01', 'week': '1', 'weekday': 'Saturday', 'close': '6262.1471'}, {'date': '2017-01-08', 'month': '01', 'week': '1', 'weekday': 'Sunday', 'close': '6319.9404'}, {'date': '2017-01-09', 'month': '01', 'week': '2', 'weekday': 'Monday', 'close': '6239.1506'}, {'date': '2017-01-10', 'month': '01', 'week': '2', 'weekday': 'Tuesday', 'close': '6263.1548'}, {'date': '2017-01-11', 'month': '01', 'week': '2', 'weekday': 'Wednesday', 'close': '5383.0598'}, {'date': '2017-01-12', 'month': '01', 'week': '2', 'weekday': 'Thursday', 'close': '5566.7345'}, {'date': '2017-01-13', 'month': '01', 'week': '2', 'weekday': 'Friday', 'close': '5700.0716'}, {'date': '2017-01-14', 'month': '01', 'week': '2', 'weekday': 'Saturday', 'close': '5648.6897'}, {'date': '2017-01-15', 'month': '01', 'week': '2', 'weekday': 'Sunday', 'close': '5674.7977'}, {'date': '2017-01-16', 'month': '01', 'week': '3', 'weekday': 'Monday', 'close': '5730.0658'}, {'date': '2017-01-17', 'month': '01', 'week': '3', 'weekday': 'Tuesday', 'close': '6202.9704'}, {'date': '2017-01-18', 'month': '01', 'week': '3', 'weekday': 'Wednesday', 'close': '6047.6601'}, {'date': '2017-01-19', 'month': '01', 'week': '3', 'weekday': 'Thursday', 'close': '6170.8433'}, {'date': '2017-01-20', 'month': '01', 'week': '3', 'weekday': 'Friday', 'close': '6131.2511'}, {'date': '2017-01-21', 'month': '01', 'week': '3', 'weekday': 'Saturday', 'close': '6326.3657'}, {'date': '2017-01-22', 'month': '01', 'week': '3', 'weekday': 'Sunday', 'close': '6362.9482'}, {'date': '2017-01-23', 'month': '01', 'week': '4', 'weekday': 'Monday', 'close': '6255.5602'}, {'date': '2017-01-24', 'month': '01', 'week': '4', 'weekday': 'Tuesday', 'close': '6074.8333'}, {'date': '2017-01-25', 'month': '01', 'week': '4', 'weekday': 'Wednesday', 'close': '6154.6958'}, {'date': '2017-01-26', 'month': '01', 'week': '4', 'weekday': 'Thursday', 'close': '6295.3388'}, {'date': '2017-01-27', 'month': '01', 'week': '4', 'weekday': 'Friday', 'close': '6320.7206'}, {'date': '2017-01-28', 'month': '01', 'week': '4', 'weekday': 'Saturday', 'close': '6332.5389'}, {'date': '2017-01-29', 'month': '01', 'week': '4', 'weekday': 'Sunday', 'close': '6289.1698'}, {'date': '2017-01-30', 'month': '01', 'week': '5', 'weekday': 'Monday', 'close': '6332.8246'}, {'date': '2017-01-31', 'month': '01', 'week': '5', 'weekday': 'Tuesday', 'close': '6657.8667'}, {'date': '2017-02-01', 'month': '02', 'week': '5', 'weekday': 'Wednesday', 'close': '6793.7077'}, {'date': '2017-02-02', 'month': '02', 'week': '5', 'weekday': 'Thursday', 'close': '6934.3856'}, {'date': '2017-02-03', 'month': '02', 'week': '5', 'weekday': 'Friday', 'close': '6995.2901'}, {'date': '2017-02-04', 'month': '02', 'week': '5', 'weekday': 'Saturday', 'close': '7102.0714'}, {'date': '2017-02-05', 'month': '02', 'week': '5', 'weekday': 'Sunday', 'close': '6965.9773'}, {'date': '2017-02-06', 'month': '02', 'week': '6', 'weekday': 'Monday', 'close': '7034.2211'}, {'date': '2017-02-07', 'month': '02', 'week': '6', 'weekday': 'Tuesday', 'close': '7245.8877'}, {'date': '2017-02-08', 'month': '02', 'week': '6', 'weekday': 'Wednesday', 'close': '7246.6303'}, {'date': '2017-02-09', 'month': '02', 'week': '6', 'weekday': 'Thursday', 'close': '6811.6794'}, {'date': '2017-02-10', 'month': '02', 'week': '6', 'weekday': 'Friday', 'close': '6833.4884'}, {'date': '2017-02-11', 'month': '02', 'week': '6', 'weekday': 'Saturday', 'close': '6946.09'}, {'date': '2017-02-12', 'month': '02', 'week': '6', 'weekday': 'Sunday', 'close': '6883.9424'}, {'date': '2017-02-13', 'month': '02', 'week': '7', 'weekday': 'Monday', 'close': '6858.5789'}, {'date': '2017-02-14', 'month': '02', 'week': '7', 'weekday': 'Tuesday', 'close': '6930.882'}, {'date': '2017-02-15', 'month': '02', 'week': '7', 'weekday': 'Wednesday', 'close': '6935.3788'}, {'date': '2017-02-16', 'month': '02', 'week': '7', 'weekday': 'Thursday', 'close': '7088.8535'}, {'date': '2017-02-17', 'month': '02', 'week': '7', 'weekday': 'Friday', 'close': '7229.5808'}, {'date': '2017-02-18', 'month': '02', 'week': '7', 'weekday': 'Saturday', 'close': '7267.5468'}, {'date': '2017-02-19', 'month': '02', 'week': '7', 'weekday': 'Sunday', 'close': '7220.5385'}, {'date': '2017-02-20', 'month': '02', 'week': '8', 'weekday': 'Monday', 'close': '7450.2901'}, {'date': '2017-02-21', 'month': '02', 'week': '8', 'weekday': 'Tuesday', 'close': '7732.4979'}, {'date': '2017-02-22', 'month': '02', 'week': '8', 'weekday': 'Wednesday', 'close': '7716.2218'}, {'date': '2017-02-23', 'month': '02', 'week': '8', 'weekday': 'Thursday', 'close': '8092.0221'}, {'date': '2017-02-24', 'month': '02', 'week': '8', 'weekday': 'Friday', 'close': '8109.1867'}, {'date': '2017-02-25', 'month': '02', 'week': '8', 'weekday': 'Saturday', 'close': '7908.54'}, {'date': '2017-02-26', 'month': '02', 'week': '8', 'weekday': 'Sunday', 'close': '8137.4131'}, {'date': '2017-02-27', 'month': '02', 'week': '9', 'weekday': 'Monday', 'close': '8206.1829'}, {'date': '2017-02-28', 'month': '02', 'week': '9', 'weekday': 'Tuesday', 'close': '8176.3692'}, {'date': '2017-03-01', 'month': '03', 'week': '9', 'weekday': 'Wednesday', 'close': '8464.3549'}, {'date': '2017-03-02', 'month': '03', 'week': '9', 'weekday': 'Thursday', 'close': '8688.7751'}, {'date': '2017-03-03', 'month': '03', 'week': '9', 'weekday': 'Friday', 'close': '8900.4858'}, {'date': '2017-03-04', 'month': '03', 'week': '9', 'weekday': 'Saturday', 'close': '8741.0338'}, {'date': '2017-03-05', 'month': '03', 'week': '9', 'weekday': 'Sunday', 'close': '8816.6651'}, {'date': '2017-03-06', 'month': '03', 'week': '10', 'weekday': 'Monday', 'close': '8832.9615'}, {'date': '2017-03-07', 'month': '03', 'week': '10', 'weekday': 'Tuesday', 'close': '8504.1113'}, {'date': '2017-03-08', 'month': '03', 'week': '10', 'weekday': 'Wednesday', 'close': '7953.9243'}, {'date': '2017-03-09', 'month': '03', 'week': '10', 'weekday': 'Thursday', 'close': '8235.799'}, {'date': '2017-03-10', 'month': '03', 'week': '10', 'weekday': 'Friday', 'close': '7716.1296'}, {'date': '2017-03-11', 'month': '03', 'week': '10', 'weekday': 'Saturday', 'close': '8161.7404'}, {'date': '2017-03-12', 'month': '03', 'week': '10', 'weekday': 'Sunday', 'close': '8441.5353'}, {'date': '2017-03-13', 'month': '03', 'week': '11', 'weekday': 'Monday', 'close': '8595.5263'}, {'date': '2017-03-14', 'month': '03', 'week': '11', 'weekday': 'Tuesday', 'close': '8616.949'}, {'date': '2017-03-15', 'month': '03', 'week': '11', 'weekday': 'Wednesday', 'close': '8711.306'}, {'date': '2017-03-16', 'month': '03', 'week': '11', 'weekday': 'Thursday', 'close': '8091.7411'}, {'date': '2017-03-17', 'month': '03', 'week': '11', 'weekday': 'Friday', 'close': '7379.6562'}, {'date': '2017-03-18', 'month': '03', 'week': '11', 'weekday': 'Saturday', 'close': '6694.36'}, {'date': '2017-03-19', 'month': '03', 'week': '11', 'weekday': 'Sunday', 'close': '7028.0107'}, {'date': '2017-03-20', 'month': '03', 'week': '12', 'weekday': 'Monday', 'close': '7196.3568'}, {'date': '2017-03-21', 'month': '03', 'week': '12', 'weekday': 'Tuesday', 'close': '7680.723'}, {'date': '2017-03-22', 'month': '03', 'week': '12', 'weekday': 'Wednesday', 'close': '7139.7016'}, {'date': '2017-03-23', 'month': '03', 'week': '12', 'weekday': 'Thursday', 'close': '7092.2246'}, {'date': '2017-03-24', 'month': '03', 'week': '12', 'weekday': 'Friday', 'close': '6437.3431'}, {'date': '2017-03-25', 'month': '03', 'week': '12', 'weekday': 'Saturday', 'close': '6640.554'}, {'date': '2017-03-26', 'month': '03', 'week': '12', 'weekday': 'Sunday', 'close': '6623.5896'}, {'date': '2017-03-27', 'month': '03', 'week': '13', 'weekday': 'Monday', 'close': '7151.8202'}, {'date': '2017-03-28', 'month': '03', 'week': '13', 'weekday': 'Tuesday', 'close': '7184.6725'}, {'date': '2017-03-29', 'month': '03', 'week': '13', 'weekday': 'Wednesday', 'close': '7168.8792'}, {'date': '2017-03-30', 'month': '03', 'week': '13', 'weekday': 'Thursday', 'close': '7146.3119'}, {'date': '2017-03-31', 'month': '03', 'week': '13', 'weekday': 'Friday', 'close': '7439.1397'}, {'date': '2017-04-01', 'month': '04', 'week': '13', 'weekday': 'Saturday', 'close': '7506.4038'}, {'date': '2017-04-02', 'month': '04', 'week': '13', 'weekday': 'Sunday', 'close': '7566.0156'}, {'date': '2017-04-03', 'month': '04', 'week': '14', 'weekday': 'Monday', 'close': '7903.6773'}, {'date': '2017-04-04', 'month': '04', 'week': '14', 'weekday': 'Tuesday', 'close': '7874.9773'}, {'date': '2017-04-05', 'month': '04', 'week': '14', 'weekday': 'Wednesday', 'close': '7827.8202'}, {'date': '2017-04-06', 'month': '04', 'week': '14', 'weekday': 'Thursday', 'close': '8212.9762'}, {'date': '2017-04-07', 'month': '04', 'week': '14', 'weekday': 'Friday', 'close': '8236.9016'}, {'date': '2017-04-08', 'month': '04', 'week': '14', 'weekday': 'Saturday', 'close': '8180.3212'}, {'date': '2017-04-09', 'month': '04', 'week': '14', 'weekday': 'Sunday', 'close': '8354.8293'}, {'date': '2017-04-10', 'month': '04', 'week': '15', 'weekday': 'Monday', 'close': '8375.1346'}, {'date': '2017-04-11', 'month': '04', 'week': '15', 'weekday': 'Tuesday', 'close': '8442.2186'}, {'date': '2017-04-12', 'month': '04', 'week': '15', 'weekday': 'Wednesday', 'close': '8382.4873'}, {'date': '2017-04-13', 'month': '04', 'week': '15', 'weekday': 'Thursday', 'close': '8117.4785'}, {'date': '2017-04-14', 'month': '04', 'week': '15', 'weekday': 'Friday', 'close': '8151.1798'}, {'date': '2017-04-15', 'month': '04', 'week': '15', 'weekday': 'Saturday', 'close': '8129.5847'}, {'date': '2017-04-16', 'month': '04', 'week': '15', 'weekday': 'Sunday', 'close': '8167.0471'}, {'date': '2017-04-17', 'month': '04', 'week': '16', 'weekday': 'Monday', 'close': '8267.104'}, {'date': '2017-04-18', 'month': '04', 'week': '16', 'weekday': 'Tuesday', 'close': '8379.7232'}, {'date': '2017-04-19', 'month': '04', 'week': '16', 'weekday': 'Wednesday', 'close': '8436.3248'}, {'date': '2017-04-20', 'month': '04', 'week': '16', 'weekday': 'Thursday', 'close': '8639.4949'}, {'date': '2017-04-21', 'month': '04', 'week': '16', 'weekday': 'Friday', 'close': '8654.9971'}, {'date': '2017-04-22', 'month': '04', 'week': '16', 'weekday': 'Saturday', 'close': '8567.1483'}, {'date': '2017-04-23', 'month': '04', 'week': '16', 'weekday': 'Sunday', 'close': '8458.4188'}, {'date': '2017-04-24', 'month': '04', 'week': '17', 'weekday': 'Monday', 'close': '8594.2345'}, {'date': '2017-04-25', 'month': '04', 'week': '17', 'weekday': 'Tuesday', 'close': '8700.0125'}, {'date': '2017-04-26', 'month': '04', 'week': '17', 'weekday': 'Wednesday', 'close': '8857.1946'}, {'date': '2017-04-27', 'month': '04', 'week': '17', 'weekday': 'Thursday', 'close': '9167.2508'}, {'date': '2017-04-28', 'month': '04', 'week': '17', 'weekday': 'Friday', 'close': '9101.0934'}, {'date': '2017-04-29', 'month': '04', 'week': '17', 'weekday': 'Saturday', 'close': '9149.9325'}, {'date': '2017-04-30', 'month': '04', 'week': '17', 'weekday': 'Sunday', 'close': '9325.1119'}, {'date': '2017-05-01', 'month': '05', 'week': '18', 'weekday': 'Monday', 'close': '9665.7551'}, {'date': '2017-05-02', 'month': '05', 'week': '18', 'weekday': 'Tuesday', 'close': '9944.3653'}, {'date': '2017-05-03', 'month': '05', 'week': '18', 'weekday': 'Wednesday', 'close': '10292.3296'}, {'date': '2017-05-04', 'month': '05', 'week': '18', 'weekday': 'Thursday', 'close': '10452.0037'}, {'date': '2017-05-05', 'month': '05', 'week': '18', 'weekday': 'Friday', 'close': '10439.0799'}, {'date': '2017-05-06', 'month': '05', 'week': '18', 'weekday': 'Saturday', 'close': '10688.1301'}, {'date': '2017-05-07', 'month': '05', 'week': '18', 'weekday': 'Sunday', 'close': '10660.1939'}, {'date': '2017-05-08', 'month': '05', 'week': '19', 'weekday': 'Monday', 'close': '11317.8009'}, {'date': '2017-05-09', 'month': '05', 'week': '19', 'weekday': 'Tuesday', 'close': '11794.8949'}, {'date': '2017-05-10', 'month': '05', 'week': '19', 'weekday': 'Wednesday', 'close': '12126.2961'}, {'date': '2017-05-11', 'month': '05', 'week': '19', 'weekday': 'Thursday', 'close': '12478.0838'}, {'date': '2017-05-12', 'month': '05', 'week': '19', 'weekday': 'Friday', 'close': '11569.4125'}, {'date': '2017-05-13', 'month': '05', 'week': '19', 'weekday': 'Saturday', 'close': '12141.797'}, {'date': '2017-05-14', 'month': '05', 'week': '19', 'weekday': 'Sunday', 'close': '12229.3176'}, {'date': '2017-05-15', 'month': '05', 'week': '20', 'weekday': 'Monday', 'close': '11701.2204'}, {'date': '2017-05-16', 'month': '05', 'week': '20', 'weekday': 'Tuesday', 'close': '11835.218'}, {'date': '2017-05-17', 'month': '05', 'week': '20', 'weekday': 'Wednesday', 'close': '12403.3024'}, {'date': '2017-05-18', 'month': '05', 'week': '20', 'weekday': 'Thursday', 'close': '13002.0625'}, {'date': '2017-05-19', 'month': '05', 'week': '20', 'weekday': 'Friday', 'close': '13549.3033'}, {'date': '2017-05-20', 'month': '05', 'week': '20', 'weekday': 'Saturday', 'close': '14127.3239'}, {'date': '2017-05-21', 'month': '05', 'week': '20', 'weekday': 'Sunday', 'close': '14091.8068'}, {'date': '2017-05-22', 'month': '05', 'week': '21', 'weekday': 'Monday', 'close': '14731.8028'}, {'date': '2017-05-23', 'month': '05', 'week': '21', 'weekday': 'Tuesday', 'close': '15784.8432'}, {'date': '2017-05-24', 'month': '05', 'week': '21', 'weekday': 'Wednesday', 'close': '17061.8818'}, {'date': '2017-05-25', 'month': '05', 'week': '21', 'weekday': 'Thursday', 'close': '16190.3931'}, {'date': '2017-05-26', 'month': '05', 'week': '21', 'weekday': 'Friday', 'close': '15402.2219'}, {'date': '2017-05-27', 'month': '05', 'week': '21', 'weekday': 'Saturday', 'close': '14440.0015'}, {'date': '2017-05-28', 'month': '05', 'week': '21', 'weekday': 'Sunday', 'close': '15139.4071'}, {'date': '2017-05-29', 'month': '05', 'week': '22', 'weekday': 'Monday', 'close': '15700.3794'}, {'date': '2017-05-30', 'month': '05', 'week': '22', 'weekday': 'Tuesday', 'close': '15064.5355'}, {'date': '2017-05-31', 'month': '05', 'week': '22', 'weekday': 'Wednesday', 'close': '15869.5798'}, {'date': '2017-06-01', 'month': '06', 'week': '22', 'weekday': 'Thursday', 'close': '16693.6332'}, {'date': '2017-06-02', 'month': '06', 'week': '22', 'weekday': 'Friday', 'close': '17149.9736'}, {'date': '2017-06-03', 'month': '06', 'week': '22', 'weekday': 'Saturday', 'close': '17410.0077'}, {'date': '2017-06-04', 'month': '06', 'week': '22', 'weekday': 'Sunday', 'close': '17399.0513'}, {'date': '2017-06-05', 'month': '06', 'week': '23', 'weekday': 'Monday', 'close': '18621.161'}, {'date': '2017-06-06', 'month': '06', 'week': '23', 'weekday': 'Tuesday', 'close': '19797.8391'}, {'date': '2017-06-07', 'month': '06', 'week': '23', 'weekday': 'Wednesday', 'close': '18205.3747'}, {'date': '2017-06-08', 'month': '06', 'week': '23', 'weekday': 'Thursday', 'close': '19209.0831'}, {'date': '2017-06-09', 'month': '06', 'week': '23', 'weekday': 'Friday', 'close': '19218.5925'}, {'date': '2017-06-10', 'month': '06', 'week': '23', 'weekday': 'Saturday', 'close': '20004.1207'}, {'date': '2017-06-11', 'month': '06', 'week': '23', 'weekday': 'Sunday', 'close': '20472.3611'}, {'date': '2017-06-12', 'month': '06', 'week': '24', 'weekday': 'Monday', 'close': '18234.4754'}, {'date': '2017-06-13', 'month': '06', 'week': '24', 'weekday': 'Tuesday', 'close': '18615.1877'}, {'date': '2017-06-14', 'month': '06', 'week': '24', 'weekday': 'Wednesday', 'close': '16946.0339'}, {'date': '2017-06-15', 'month': '06', 'week': '24', 'weekday': 'Thursday', 'close': '16724.4891'}, {'date': '2017-06-16', 'month': '06', 'week': '24', 'weekday': 'Friday', 'close': '17217.0095'}, {'date': '2017-06-17', 'month': '06', 'week': '24', 'weekday': 'Saturday', 'close': '18142.6219'}, {'date': '2017-06-18', 'month': '06', 'week': '24', 'weekday': 'Sunday', 'close': '17535.8535'}, {'date': '2017-06-19', 'month': '06', 'week': '25', 'weekday': 'Monday', 'close': '18015.1039'}, {'date': '2017-06-20', 'month': '06', 'week': '25', 'weekday': 'Tuesday', 'close': '18975.7796'}, {'date': '2017-06-21', 'month': '06', 'week': '25', 'weekday': 'Wednesday', 'close': '18522.6802'}, {'date': '2017-06-22', 'month': '06', 'week': '25', 'weekday': 'Thursday', 'close': '18733.2802'}, {'date': '2017-06-23', 'month': '06', 'week': '25', 'weekday': 'Friday', 'close': '18720.4229'}, {'date': '2017-06-24', 'month': '06', 'week': '25', 'weekday': 'Saturday', 'close': '17906.1295'}, {'date': '2017-06-25', 'month': '06', 'week': '25', 'weekday': 'Sunday', 'close': '17734.3884'}, {'date': '2017-06-26', 'month': '06', 'week': '26', 'weekday': 'Monday', 'close': '17001.592'}, {'date': '2017-06-27', 'month': '06', 'week': '26', 'weekday': 'Tuesday', 'close': '17666.3417'}, {'date': '2017-06-28', 'month': '06', 'week': '26', 'weekday': 'Wednesday', 'close': '17575.261'}, {'date': '2017-06-29', 'month': '06', 'week': '26', 'weekday': 'Thursday', 'close': '17385.3171'}, {'date': '2017-06-30', 'month': '06', 'week': '26', 'weekday': 'Friday', 'close': '16943.0147'}, {'date': '2017-07-01', 'month': '07', 'week': '26', 'weekday': 'Saturday', 'close': '16674.129'}, {'date': '2017-07-02', 'month': '07', 'week': '26', 'weekday': 'Sunday', 'close': '17150.7103'}, {'date': '2017-07-03', 'month': '07', 'week': '27', 'weekday': 'Monday', 'close': '17549.3179'}, {'date': '2017-07-04', 'month': '07', 'week': '27', 'weekday': 'Tuesday', 'close': '17851.5456'}, {'date': '2017-07-05', 'month': '07', 'week': '27', 'weekday': 'Wednesday', 'close': '17812.6481'}, {'date': '2017-07-06', 'month': '07', 'week': '27', 'weekday': 'Thursday', 'close': '17813.1077'}, {'date': '2017-07-07', 'month': '07', 'week': '27', 'weekday': 'Friday', 'close': '17156.6351'}, {'date': '2017-07-08', 'month': '07', 'week': '27', 'weekday': 'Saturday', 'close': '17557.352'}, {'date': '2017-07-09', 'month': '07', 'week': '27', 'weekday': 'Sunday', 'close': '17189.5013'}, {'date': '2017-07-10', 'month': '07', 'week': '28', 'weekday': 'Monday', 'close': '16137.2933'}, {'date': '2017-07-11', 'month': '07', 'week': '28', 'weekday': 'Tuesday', 'close': '15865.7291'}, {'date': '2017-07-12', 'month': '07', 'week': '28', 'weekday': 'Wednesday', 'close': '16446.9487'}, {'date': '2017-07-13', 'month': '07', 'week': '28', 'weekday': 'Thursday', 'close': '16036.6222'}, {'date': '2017-07-14', 'month': '07', 'week': '28', 'weekday': 'Friday', 'close': '15132.8235'}, {'date': '2017-07-15', 'month': '07', 'week': '28', 'weekday': 'Saturday', 'close': '13510.2081'}, {'date': '2017-07-16', 'month': '07', 'week': '28', 'weekday': 'Sunday', 'close': '13075.9378'}, {'date': '2017-07-17', 'month': '07', 'week': '29', 'weekday': 'Monday', 'close': '15192.6798'}, {'date': '2017-07-18', 'month': '07', 'week': '29', 'weekday': 'Tuesday', 'close': '15706.0407'}, {'date': '2017-07-19', 'month': '07', 'week': '29', 'weekday': 'Wednesday', 'close': '15491.8987'}, {'date': '2017-07-20', 'month': '07', 'week': '29', 'weekday': 'Thursday', 'close': '19449.5488'}, {'date': '2017-07-21', 'month': '07', 'week': '29', 'weekday': 'Friday', 'close': '18231.0571'}, {'date': '2017-07-22', 'month': '07', 'week': '29', 'weekday': 'Saturday', 'close': '19210.2278'}, {'date': '2017-07-23', 'month': '07', 'week': '29', 'weekday': 'Sunday', 'close': '18585.2759'}, {'date': '2017-07-24', 'month': '07', 'week': '30', 'weekday': 'Monday', 'close': '18762.6589'}, {'date': '2017-07-25', 'month': '07', 'week': '30', 'weekday': 'Tuesday', 'close': '17489.6893'}, {'date': '2017-07-26', 'month': '07', 'week': '30', 'weekday': 'Wednesday', 'close': '17219.7355'}, {'date': '2017-07-27', 'month': '07', 'week': '30', 'weekday': 'Thursday', 'close': '18188.4669'}, {'date': '2017-07-28', 'month': '07', 'week': '30', 'weekday': 'Friday', 'close': '18898.2088'}, {'date': '2017-07-29', 'month': '07', 'week': '30', 'weekday': 'Saturday', 'close': '18326.2673'}, {'date': '2017-07-30', 'month': '07', 'week': '30', 'weekday': 'Sunday', 'close': '18499.4572'}, {'date': '2017-07-31', 'month': '07', 'week': '31', 'weekday': 'Monday', 'close': '19334.0151'}, {'date': '2017-08-01', 'month': '08', 'week': '31', 'weekday': 'Tuesday', 'close': '18376.5093'}, {'date': '2017-08-02', 'month': '08', 'week': '31', 'weekday': 'Wednesday', 'close': '18305.9985'}, {'date': '2017-08-03', 'month': '08', 'week': '31', 'weekday': 'Thursday', 'close': '18901.8131'}, {'date': '2017-08-04', 'month': '08', 'week': '31', 'weekday': 'Friday', 'close': '19412.7978'}, {'date': '2017-08-05', 'month': '08', 'week': '31', 'weekday': 'Saturday', 'close': '22227.2745'}, {'date': '2017-08-06', 'month': '08', 'week': '31', 'weekday': 'Sunday', 'close': '22027.9885'}, {'date': '2017-08-07', 'month': '08', 'week': '32', 'weekday': 'Monday', 'close': '23063.1732'}, {'date': '2017-08-08', 'month': '08', 'week': '32', 'weekday': 'Tuesday', 'close': '23332.6502'}, {'date': '2017-08-09', 'month': '08', 'week': '32', 'weekday': 'Wednesday', 'close': '22541.6557'}, {'date': '2017-08-10', 'month': '08', 'week': '32', 'weekday': 'Thursday', 'close': '22904.393'}, {'date': '2017-08-11', 'month': '08', 'week': '32', 'weekday': 'Friday', 'close': '24526.2402'}, {'date': '2017-08-12', 'month': '08', 'week': '32', 'weekday': 'Saturday', 'close': '26109.954'}, {'date': '2017-08-13', 'month': '08', 'week': '32', 'weekday': 'Sunday', 'close': '27390.8412'}, {'date': '2017-08-14', 'month': '08', 'week': '33', 'weekday': 'Monday', 'close': '29237.7752'}, {'date': '2017-08-15', 'month': '08', 'week': '33', 'weekday': 'Tuesday', 'close': '28073.6691'}, {'date': '2017-08-16', 'month': '08', 'week': '33', 'weekday': 'Wednesday', 'close': '29612.7553'}, {'date': '2017-08-17', 'month': '08', 'week': '33', 'weekday': 'Thursday', 'close': '28816.4854'}, {'date': '2017-08-18', 'month': '08', 'week': '33', 'weekday': 'Friday', 'close': '27752.9395'}, {'date': '2017-08-19', 'month': '08', 'week': '33', 'weekday': 'Saturday', 'close': '28062.8866'}, {'date': '2017-08-20', 'month': '08', 'week': '33', 'weekday': 'Sunday', 'close': '27416.633'}, {'date': '2017-08-21', 'month': '08', 'week': '34', 'weekday': 'Monday', 'close': '26919.0691'}, {'date': '2017-08-22', 'month': '08', 'week': '34', 'weekday': 'Tuesday', 'close': '27565.3902'}, {'date': '2017-08-23', 'month': '08', 'week': '34', 'weekday': 'Wednesday', 'close': '27907.3345'}, {'date': '2017-08-24', 'month': '08', 'week': '34', 'weekday': 'Thursday', 'close': '29063.2438'}, {'date': '2017-08-25', 'month': '08', 'week': '34', 'weekday': 'Friday', 'close': '29305.655'}, {'date': '2017-08-26', 'month': '08', 'week': '34', 'weekday': 'Saturday', 'close': '29168.7202'}, {'date': '2017-08-27', 'month': '08', 'week': '34', 'weekday': 'Sunday', 'close': '28945.352'}, {'date': '2017-08-28', 'month': '08', 'week': '35', 'weekday': 'Monday', 'close': '29340.152'}, {'date': '2017-08-29', 'month': '08', 'week': '35', 'weekday': 'Tuesday', 'close': '30656.5006'}, {'date': '2017-08-30', 'month': '08', 'week': '35', 'weekday': 'Wednesday', 'close': '30532.943'}, {'date': '2017-08-31', 'month': '08', 'week': '35', 'weekday': 'Thursday', 'close': '31391.9005'}, {'date': '2017-09-01', 'month': '09', 'week': '35', 'weekday': 'Friday', 'close': '32482.9375'}, {'date': '2017-09-02', 'month': '09', 'week': '35', 'weekday': 'Saturday', 'close': '30470.2819'}, {'date': '2017-09-03', 'month': '09', 'week': '35', 'weekday': 'Sunday', 'close': '30336.7'}, {'date': '2017-09-04', 'month': '09', 'week': '36', 'weekday': 'Monday', 'close': '28208.2898'}, {'date': '2017-09-05', 'month': '09', 'week': '36', 'weekday': 'Tuesday', 'close': '28918.0205'}, {'date': '2017-09-06', 'month': '09', 'week': '36', 'weekday': 'Wednesday', 'close': '30181.6445'}, {'date': '2017-09-07', 'month': '09', 'week': '36', 'weekday': 'Thursday', 'close': '30089.0416'}, {'date': '2017-09-08', 'month': '09', 'week': '36', 'weekday': 'Friday', 'close': '27976.5645'}, {'date': '2017-09-09', 'month': '09', 'week': '36', 'weekday': 'Saturday', 'close': '27818.6782'}, {'date': '2017-09-10', 'month': '09', 'week': '36', 'weekday': 'Sunday', 'close': '27359.9317'}, {'date': '2017-09-11', 'month': '09', 'week': '37', 'weekday': 'Monday', 'close': '27351.0555'}, {'date': '2017-09-12', 'month': '09', 'week': '37', 'weekday': 'Tuesday', 'close': '27111.147'}, {'date': '2017-09-13', 'month': '09', 'week': '37', 'weekday': 'Wednesday', 'close': '25354.506'}, {'date': '2017-09-14', 'month': '09', 'week': '37', 'weekday': 'Thursday', 'close': '21152.8443'}, {'date': '2017-09-15', 'month': '09', 'week': '37', 'weekday': 'Friday', 'close': '24164.8636'}, {'date': '2017-09-16', 'month': '09', 'week': '37', 'weekday': 'Saturday', 'close': '24111.3645'}, {'date': '2017-09-17', 'month': '09', 'week': '37', 'weekday': 'Sunday', 'close': '24057.8213'}, {'date': '2017-09-18', 'month': '09', 'week': '38', 'weekday': 'Monday', 'close': '26737.3742'}, {'date': '2017-09-19', 'month': '09', 'week': '38', 'weekday': 'Tuesday', 'close': '25652.4813'}, {'date': '2017-09-20', 'month': '09', 'week': '38', 'weekday': 'Wednesday', 'close': '25361.3238'}, {'date': '2017-09-21', 'month': '09', 'week': '38', 'weekday': 'Thursday', 'close': '23804.5608'}, {'date': '2017-09-22', 'month': '09', 'week': '38', 'weekday': 'Friday', 'close': '23761.1198'}, {'date': '2017-09-23', 'month': '09', 'week': '38', 'weekday': 'Saturday', 'close': '24908.4204'}, {'date': '2017-09-24', 'month': '09', 'week': '38', 'weekday': 'Sunday', 'close': '24216.5269'}, {'date': '2017-09-25', 'month': '09', 'week': '39', 'weekday': 'Monday', 'close': '26007.1112'}, {'date': '2017-09-26', 'month': '09', 'week': '39', 'weekday': 'Tuesday', 'close': '25869.3194'}, {'date': '2017-09-27', 'month': '09', 'week': '39', 'weekday': 'Wednesday', 'close': '27955.6252'}, {'date': '2017-09-28', 'month': '09', 'week': '39', 'weekday': 'Thursday', 'close': '27882.4195'}, {'date': '2017-09-29', 'month': '09', 'week': '39', 'weekday': 'Friday', 'close': '27711.6948'}, {'date': '2017-09-30', 'month': '09', 'week': '39', 'weekday': 'Saturday', 'close': '28969.0962'}, {'date': '2017-10-01', 'month': '10', 'week': '39', 'weekday': 'Sunday', 'close': '29264.4926'}, {'date': '2017-10-02', 'month': '10', 'week': '40', 'weekday': 'Monday', 'close': '29295.7562'}, {'date': '2017-10-03', 'month': '10', 'week': '40', 'weekday': 'Tuesday', 'close': '28743.0928'}, {'date': '2017-10-04', 'month': '10', 'week': '40', 'weekday': 'Wednesday', 'close': '28120.5656'}, {'date': '2017-10-05', 'month': '10', 'week': '40', 'weekday': 'Thursday', 'close': '28764.0436'}, {'date': '2017-10-06', 'month': '10', 'week': '40', 'weekday': 'Friday', 'close': '29084.1981'}, {'date': '2017-10-07', 'month': '10', 'week': '40', 'weekday': 'Saturday', 'close': '29521.3602'}, {'date': '2017-10-08', 'month': '10', 'week': '40', 'weekday': 'Sunday', 'close': '30583.2886'}, {'date': '2017-10-09', 'month': '10', 'week': '41', 'weekday': 'Monday', 'close': '31622.869'}, {'date': '2017-10-10', 'month': '10', 'week': '41', 'weekday': 'Tuesday', 'close': '31243.3645'}, {'date': '2017-10-11', 'month': '10', 'week': '41', 'weekday': 'Wednesday', 'close': '31830.4848'}, {'date': '2017-10-12', 'month': '10', 'week': '41', 'weekday': 'Thursday', 'close': '35833.2539'}, {'date': '2017-10-13', 'month': '10', 'week': '41', 'weekday': 'Friday', 'close': '37106.6814'}, {'date': '2017-10-14', 'month': '10', 'week': '41', 'weekday': 'Saturday', 'close': '38222.2666'}, {'date': '2017-10-15', 'month': '10', 'week': '41', 'weekday': 'Sunday', 'close': '37517.0856'}, {'date': '2017-10-16', 'month': '10', 'week': '42', 'weekday': 'Monday', 'close': '37917.9925'}, {'date': '2017-10-17', 'month': '10', 'week': '42', 'weekday': 'Tuesday', 'close': '37060.3182'}, {'date': '2017-10-18', 'month': '10', 'week': '42', 'weekday': 'Wednesday', 'close': '36928.632'}, {'date': '2017-10-19', 'month': '10', 'week': '42', 'weekday': 'Thursday', 'close': '37704.4579'}, {'date': '2017-10-20', 'month': '10', 'week': '42', 'weekday': 'Friday', 'close': '39634.3994'}, {'date': '2017-10-21', 'month': '10', 'week': '42', 'weekday': 'Saturday', 'close': '39827.4189'}, {'date': '2017-10-22', 'month': '10', 'week': '42', 'weekday': 'Sunday', 'close': '39673.3776'}, {'date': '2017-10-23', 'month': '10', 'week': '43', 'weekday': 'Monday', 'close': '39144.7834'}, {'date': '2017-10-24', 'month': '10', 'week': '43', 'weekday': 'Tuesday', 'close': '36611.829'}, {'date': '2017-10-25', 'month': '10', 'week': '43', 'weekday': 'Wednesday', 'close': '38067.9613'}, {'date': '2017-10-26', 'month': '10', 'week': '43', 'weekday': 'Thursday', 'close': '39108.6475'}, {'date': '2017-10-27', 'month': '10', 'week': '43', 'weekday': 'Friday', 'close': '38353.9173'}, {'date': '2017-10-28', 'month': '10', 'week': '43', 'weekday': 'Saturday', 'close': '38122.1385'}, {'date': '2017-10-29', 'month': '10', 'week': '43', 'weekday': 'Sunday', 'close': '40925.4142'}, {'date': '2017-10-30', 'month': '10', 'week': '44', 'weekday': 'Monday', 'close': '40682.7268'}, {'date': '2017-10-31', 'month': '10', 'week': '44', 'weekday': 'Tuesday', 'close': '42779.3067'}, {'date': '2017-11-01', 'month': '11', 'week': '44', 'weekday': 'Wednesday', 'close': '44572.0627'}, {'date': '2017-11-02', 'month': '11', 'week': '44', 'weekday': 'Thursday', 'close': '46462.0654'}, {'date': '2017-11-03', 'month': '11', 'week': '44', 'weekday': 'Friday', 'close': '47518.0205'}, {'date': '2017-11-04', 'month': '11', 'week': '44', 'weekday': 'Saturday', 'close': '49047.8253'}, {'date': '2017-11-05', 'month': '11', 'week': '44', 'weekday': 'Sunday', 'close': '48907.9843'}, {'date': '2017-11-06', 'month': '11', 'week': '45', 'weekday': 'Monday', 'close': '46159.7307'}, {'date': '2017-11-07', 'month': '11', 'week': '45', 'weekday': 'Tuesday', 'close': '47249.3415'}, {'date': '2017-11-08', 'month': '11', 'week': '45', 'weekday': 'Wednesday', 'close': '49427.7048'}, {'date': '2017-11-09', 'month': '11', 'week': '45', 'weekday': 'Thursday', 'close': '47448.9118'}, {'date': '2017-11-10', 'month': '11', 'week': '45', 'weekday': 'Friday', 'close': '43637.0596'}, {'date': '2017-11-11', 'month': '11', 'week': '45', 'weekday': 'Saturday', 'close': '42085.6019'}, {'date': '2017-11-12', 'month': '11', 'week': '45', 'weekday': 'Sunday', 'close': '38904.304'}, {'date': '2017-11-13', 'month': '11', 'week': '46', 'weekday': 'Monday', 'close': '43279.9771'}, {'date': '2017-11-14', 'month': '11', 'week': '46', 'weekday': 'Tuesday', 'close': '43801.9668'}, {'date': '2017-11-15', 'month': '11', 'week': '46', 'weekday': 'Wednesday', 'close': '48232.4671'}, {'date': '2017-11-16', 'month': '11', 'week': '46', 'weekday': 'Thursday', 'close': '52012.2859'}, {'date': '2017-11-17', 'month': '11', 'week': '46', 'weekday': 'Friday', 'close': '50973.7739'}, {'date': '2017-11-18', 'month': '11', 'week': '46', 'weekday': 'Saturday', 'close': '51542.0595'}, {'date': '2017-11-19', 'month': '11', 'week': '46', 'weekday': 'Sunday', 'close': '53279.4604'}, {'date': '2017-11-20', 'month': '11', 'week': '47', 'weekday': 'Monday', 'close': '54656.3545'}, {'date': '2017-11-21', 'month': '11', 'week': '47', 'weekday': 'Tuesday', 'close': '53673.7877'}, {'date': '2017-11-22', 'month': '11', 'week': '47', 'weekday': 'Wednesday', 'close': '54403.2305'}, {'date': '2017-11-23', 'month': '11', 'week': '47', 'weekday': 'Thursday', 'close': '52676.5845'}, {'date': '2017-11-24', 'month': '11', 'week': '47', 'weekday': 'Friday', 'close': '54136.6166'}, {'date': '2017-11-25', 'month': '11', 'week': '47', 'weekday': 'Saturday', 'close': '57851.0594'}, {'date': '2017-11-26', 'month': '11', 'week': '47', 'weekday': 'Sunday', 'close': '60980.0178'}, {'date': '2017-11-27', 'month': '11', 'week': '48', 'weekday': 'Monday', 'close': '64246.5971'}, {'date': '2017-11-28', 'month': '11', 'week': '48', 'weekday': 'Tuesday', 'close': '65458.8612'}, {'date': '2017-11-29', 'month': '11', 'week': '48', 'weekday': 'Wednesday', 'close': '64890.9642'}, {'date': '2017-11-30', 'month': '11', 'week': '48', 'weekday': 'Thursday', 'close': '65583.2597'}, {'date': '2017-12-01', 'month': '12', 'week': '48', 'weekday': 'Friday', 'close': '71825.6883'}, {'date': '2017-12-02', 'month': '12', 'week': '48', 'weekday': 'Saturday', 'close': '72079.2312'}, {'date': '2017-12-03', 'month': '12', 'week': '48', 'weekday': 'Sunday', 'close': '74007.4136'}, {'date': '2017-12-04', 'month': '12', 'week': '49', 'weekday': 'Monday', 'close': '76852.0129'}, {'date': '2017-12-05', 'month': '12', 'week': '49', 'weekday': 'Tuesday', 'close': '77398.8752'}, {'date': '2017-12-06', 'month': '12', 'week': '49', 'weekday': 'Wednesday', 'close': '90679.5487'}, {'date': '2017-12-07', 'month': '12', 'week': '49', 'weekday': 'Thursday', 'close': '111589.9776'}, {'date': '2017-12-08', 'month': '12', 'week': '49', 'weekday': 'Friday', 'close': '106233.201'}, {'date': '2017-12-09', 'month': '12', 'week': '49', 'weekday': 'Saturday', 'close': '98676.7747'}, {'date': '2017-12-10', 'month': '12', 'week': '49', 'weekday': 'Sunday', 'close': '99525.1027'}, {'date': '2017-12-11', 'month': '12', 'week': '50', 'weekday': 'Monday', 'close': '110642.88'}, {'date': '2017-12-12', 'month': '12', 'week': '50', 'weekday': 'Tuesday', 'close': '113732.6745'}]
    


```python
print(req.decode('utf8'))
```

    [{
        "date": "2017-01-01",
        "month": "01",
        "week": "52",
        "weekday": "Sunday",
        "close": "6928.6492"
    },
    {
        "date": "2017-01-02",
        "month": "01",
        "week": "1",
        "weekday": "Monday",
        "close": "7070.2554"
    },
    {
        "date": "2017-01-03",
        "month": "01",
        "week": "1",
        "weekday": "Tuesday",
        "close": "7175.1082"
    },
    {
        "date": "2017-01-04",
        "month": "01",
        "week": "1",
        "weekday": "Wednesday",
        "close": "7835.7615"
    },
    {
        "date": "2017-01-05",
        "month": "01",
        "week": "1",
        "weekday": "Thursday",
        "close": "6928.7578"
    },
    {
        "date": "2017-01-06",
        "month": "01",
        "week": "1",
        "weekday": "Friday",
        "close": "6196.6928"
    },
    {
        "date": "2017-01-07",
        "month": "01",
        "week": "1",
        "weekday": "Saturday",
        "close": "6262.1471"
    },
    {
        "date": "2017-01-08",
        "month": "01",
        "week": "1",
        "weekday": "Sunday",
        "close": "6319.9404"
    },
    {
        "date": "2017-01-09",
        "month": "01",
        "week": "2",
        "weekday": "Monday",
        "close": "6239.1506"
    },
    {
        "date": "2017-01-10",
        "month": "01",
        "week": "2",
        "weekday": "Tuesday",
        "close": "6263.1548"
    },
    {
        "date": "2017-01-11",
        "month": "01",
        "week": "2",
        "weekday": "Wednesday",
        "close": "5383.0598"
    },
    {
        "date": "2017-01-12",
        "month": "01",
        "week": "2",
        "weekday": "Thursday",
        "close": "5566.7345"
    },
    {
        "date": "2017-01-13",
        "month": "01",
        "week": "2",
        "weekday": "Friday",
        "close": "5700.0716"
    },
    {
        "date": "2017-01-14",
        "month": "01",
        "week": "2",
        "weekday": "Saturday",
        "close": "5648.6897"
    },
    {
        "date": "2017-01-15",
        "month": "01",
        "week": "2",
        "weekday": "Sunday",
        "close": "5674.7977"
    },
    {
        "date": "2017-01-16",
        "month": "01",
        "week": "3",
        "weekday": "Monday",
        "close": "5730.0658"
    },
    {
        "date": "2017-01-17",
        "month": "01",
        "week": "3",
        "weekday": "Tuesday",
        "close": "6202.9704"
    },
    {
        "date": "2017-01-18",
        "month": "01",
        "week": "3",
        "weekday": "Wednesday",
        "close": "6047.6601"
    },
    {
        "date": "2017-01-19",
        "month": "01",
        "week": "3",
        "weekday": "Thursday",
        "close": "6170.8433"
    },
    {
        "date": "2017-01-20",
        "month": "01",
        "week": "3",
        "weekday": "Friday",
        "close": "6131.2511"
    },
    {
        "date": "2017-01-21",
        "month": "01",
        "week": "3",
        "weekday": "Saturday",
        "close": "6326.3657"
    },
    {
        "date": "2017-01-22",
        "month": "01",
        "week": "3",
        "weekday": "Sunday",
        "close": "6362.9482"
    },
    {
        "date": "2017-01-23",
        "month": "01",
        "week": "4",
        "weekday": "Monday",
        "close": "6255.5602"
    },
    {
        "date": "2017-01-24",
        "month": "01",
        "week": "4",
        "weekday": "Tuesday",
        "close": "6074.8333"
    },
    {
        "date": "2017-01-25",
        "month": "01",
        "week": "4",
        "weekday": "Wednesday",
        "close": "6154.6958"
    },
    {
        "date": "2017-01-26",
        "month": "01",
        "week": "4",
        "weekday": "Thursday",
        "close": "6295.3388"
    },
    {
        "date": "2017-01-27",
        "month": "01",
        "week": "4",
        "weekday": "Friday",
        "close": "6320.7206"
    },
    {
        "date": "2017-01-28",
        "month": "01",
        "week": "4",
        "weekday": "Saturday",
        "close": "6332.5389"
    },
    {
        "date": "2017-01-29",
        "month": "01",
        "week": "4",
        "weekday": "Sunday",
        "close": "6289.1698"
    },
    {
        "date": "2017-01-30",
        "month": "01",
        "week": "5",
        "weekday": "Monday",
        "close": "6332.8246"
    },
    {
        "date": "2017-01-31",
        "month": "01",
        "week": "5",
        "weekday": "Tuesday",
        "close": "6657.8667"
    },
    {
        "date": "2017-02-01",
        "month": "02",
        "week": "5",
        "weekday": "Wednesday",
        "close": "6793.7077"
    },
    {
        "date": "2017-02-02",
        "month": "02",
        "week": "5",
        "weekday": "Thursday",
        "close": "6934.3856"
    },
    {
        "date": "2017-02-03",
        "month": "02",
        "week": "5",
        "weekday": "Friday",
        "close": "6995.2901"
    },
    {
        "date": "2017-02-04",
        "month": "02",
        "week": "5",
        "weekday": "Saturday",
        "close": "7102.0714"
    },
    {
        "date": "2017-02-05",
        "month": "02",
        "week": "5",
        "weekday": "Sunday",
        "close": "6965.9773"
    },
    {
        "date": "2017-02-06",
        "month": "02",
        "week": "6",
        "weekday": "Monday",
        "close": "7034.2211"
    },
    {
        "date": "2017-02-07",
        "month": "02",
        "week": "6",
        "weekday": "Tuesday",
        "close": "7245.8877"
    },
    {
        "date": "2017-02-08",
        "month": "02",
        "week": "6",
        "weekday": "Wednesday",
        "close": "7246.6303"
    },
    {
        "date": "2017-02-09",
        "month": "02",
        "week": "6",
        "weekday": "Thursday",
        "close": "6811.6794"
    },
    {
        "date": "2017-02-10",
        "month": "02",
        "week": "6",
        "weekday": "Friday",
        "close": "6833.4884"
    },
    {
        "date": "2017-02-11",
        "month": "02",
        "week": "6",
        "weekday": "Saturday",
        "close": "6946.09"
    },
    {
        "date": "2017-02-12",
        "month": "02",
        "week": "6",
        "weekday": "Sunday",
        "close": "6883.9424"
    },
    {
        "date": "2017-02-13",
        "month": "02",
        "week": "7",
        "weekday": "Monday",
        "close": "6858.5789"
    },
    {
        "date": "2017-02-14",
        "month": "02",
        "week": "7",
        "weekday": "Tuesday",
        "close": "6930.882"
    },
    {
        "date": "2017-02-15",
        "month": "02",
        "week": "7",
        "weekday": "Wednesday",
        "close": "6935.3788"
    },
    {
        "date": "2017-02-16",
        "month": "02",
        "week": "7",
        "weekday": "Thursday",
        "close": "7088.8535"
    },
    {
        "date": "2017-02-17",
        "month": "02",
        "week": "7",
        "weekday": "Friday",
        "close": "7229.5808"
    },
    {
        "date": "2017-02-18",
        "month": "02",
        "week": "7",
        "weekday": "Saturday",
        "close": "7267.5468"
    },
    {
        "date": "2017-02-19",
        "month": "02",
        "week": "7",
        "weekday": "Sunday",
        "close": "7220.5385"
    },
    {
        "date": "2017-02-20",
        "month": "02",
        "week": "8",
        "weekday": "Monday",
        "close": "7450.2901"
    },
    {
        "date": "2017-02-21",
        "month": "02",
        "week": "8",
        "weekday": "Tuesday",
        "close": "7732.4979"
    },
    {
        "date": "2017-02-22",
        "month": "02",
        "week": "8",
        "weekday": "Wednesday",
        "close": "7716.2218"
    },
    {
        "date": "2017-02-23",
        "month": "02",
        "week": "8",
        "weekday": "Thursday",
        "close": "8092.0221"
    },
    {
        "date": "2017-02-24",
        "month": "02",
        "week": "8",
        "weekday": "Friday",
        "close": "8109.1867"
    },
    {
        "date": "2017-02-25",
        "month": "02",
        "week": "8",
        "weekday": "Saturday",
        "close": "7908.54"
    },
    {
        "date": "2017-02-26",
        "month": "02",
        "week": "8",
        "weekday": "Sunday",
        "close": "8137.4131"
    },
    {
        "date": "2017-02-27",
        "month": "02",
        "week": "9",
        "weekday": "Monday",
        "close": "8206.1829"
    },
    {
        "date": "2017-02-28",
        "month": "02",
        "week": "9",
        "weekday": "Tuesday",
        "close": "8176.3692"
    },
    {
        "date": "2017-03-01",
        "month": "03",
        "week": "9",
        "weekday": "Wednesday",
        "close": "8464.3549"
    },
    {
        "date": "2017-03-02",
        "month": "03",
        "week": "9",
        "weekday": "Thursday",
        "close": "8688.7751"
    },
    {
        "date": "2017-03-03",
        "month": "03",
        "week": "9",
        "weekday": "Friday",
        "close": "8900.4858"
    },
    {
        "date": "2017-03-04",
        "month": "03",
        "week": "9",
        "weekday": "Saturday",
        "close": "8741.0338"
    },
    {
        "date": "2017-03-05",
        "month": "03",
        "week": "9",
        "weekday": "Sunday",
        "close": "8816.6651"
    },
    {
        "date": "2017-03-06",
        "month": "03",
        "week": "10",
        "weekday": "Monday",
        "close": "8832.9615"
    },
    {
        "date": "2017-03-07",
        "month": "03",
        "week": "10",
        "weekday": "Tuesday",
        "close": "8504.1113"
    },
    {
        "date": "2017-03-08",
        "month": "03",
        "week": "10",
        "weekday": "Wednesday",
        "close": "7953.9243"
    },
    {
        "date": "2017-03-09",
        "month": "03",
        "week": "10",
        "weekday": "Thursday",
        "close": "8235.799"
    },
    {
        "date": "2017-03-10",
        "month": "03",
        "week": "10",
        "weekday": "Friday",
        "close": "7716.1296"
    },
    {
        "date": "2017-03-11",
        "month": "03",
        "week": "10",
        "weekday": "Saturday",
        "close": "8161.7404"
    },
    {
        "date": "2017-03-12",
        "month": "03",
        "week": "10",
        "weekday": "Sunday",
        "close": "8441.5353"
    },
    {
        "date": "2017-03-13",
        "month": "03",
        "week": "11",
        "weekday": "Monday",
        "close": "8595.5263"
    },
    {
        "date": "2017-03-14",
        "month": "03",
        "week": "11",
        "weekday": "Tuesday",
        "close": "8616.949"
    },
    {
        "date": "2017-03-15",
        "month": "03",
        "week": "11",
        "weekday": "Wednesday",
        "close": "8711.306"
    },
    {
        "date": "2017-03-16",
        "month": "03",
        "week": "11",
        "weekday": "Thursday",
        "close": "8091.7411"
    },
    {
        "date": "2017-03-17",
        "month": "03",
        "week": "11",
        "weekday": "Friday",
        "close": "7379.6562"
    },
    {
        "date": "2017-03-18",
        "month": "03",
        "week": "11",
        "weekday": "Saturday",
        "close": "6694.36"
    },
    {
        "date": "2017-03-19",
        "month": "03",
        "week": "11",
        "weekday": "Sunday",
        "close": "7028.0107"
    },
    {
        "date": "2017-03-20",
        "month": "03",
        "week": "12",
        "weekday": "Monday",
        "close": "7196.3568"
    },
    {
        "date": "2017-03-21",
        "month": "03",
        "week": "12",
        "weekday": "Tuesday",
        "close": "7680.723"
    },
    {
        "date": "2017-03-22",
        "month": "03",
        "week": "12",
        "weekday": "Wednesday",
        "close": "7139.7016"
    },
    {
        "date": "2017-03-23",
        "month": "03",
        "week": "12",
        "weekday": "Thursday",
        "close": "7092.2246"
    },
    {
        "date": "2017-03-24",
        "month": "03",
        "week": "12",
        "weekday": "Friday",
        "close": "6437.3431"
    },
    {
        "date": "2017-03-25",
        "month": "03",
        "week": "12",
        "weekday": "Saturday",
        "close": "6640.554"
    },
    {
        "date": "2017-03-26",
        "month": "03",
        "week": "12",
        "weekday": "Sunday",
        "close": "6623.5896"
    },
    {
        "date": "2017-03-27",
        "month": "03",
        "week": "13",
        "weekday": "Monday",
        "close": "7151.8202"
    },
    {
        "date": "2017-03-28",
        "month": "03",
        "week": "13",
        "weekday": "Tuesday",
        "close": "7184.6725"
    },
    {
        "date": "2017-03-29",
        "month": "03",
        "week": "13",
        "weekday": "Wednesday",
        "close": "7168.8792"
    },
    {
        "date": "2017-03-30",
        "month": "03",
        "week": "13",
        "weekday": "Thursday",
        "close": "7146.3119"
    },
    {
        "date": "2017-03-31",
        "month": "03",
        "week": "13",
        "weekday": "Friday",
        "close": "7439.1397"
    },
    {
        "date": "2017-04-01",
        "month": "04",
        "week": "13",
        "weekday": "Saturday",
        "close": "7506.4038"
    },
    {
        "date": "2017-04-02",
        "month": "04",
        "week": "13",
        "weekday": "Sunday",
        "close": "7566.0156"
    },
    {
        "date": "2017-04-03",
        "month": "04",
        "week": "14",
        "weekday": "Monday",
        "close": "7903.6773"
    },
    {
        "date": "2017-04-04",
        "month": "04",
        "week": "14",
        "weekday": "Tuesday",
        "close": "7874.9773"
    },
    {
        "date": "2017-04-05",
        "month": "04",
        "week": "14",
        "weekday": "Wednesday",
        "close": "7827.8202"
    },
    {
        "date": "2017-04-06",
        "month": "04",
        "week": "14",
        "weekday": "Thursday",
        "close": "8212.9762"
    },
    {
        "date": "2017-04-07",
        "month": "04",
        "week": "14",
        "weekday": "Friday",
        "close": "8236.9016"
    },
    {
        "date": "2017-04-08",
        "month": "04",
        "week": "14",
        "weekday": "Saturday",
        "close": "8180.3212"
    },
    {
        "date": "2017-04-09",
        "month": "04",
        "week": "14",
        "weekday": "Sunday",
        "close": "8354.8293"
    },
    {
        "date": "2017-04-10",
        "month": "04",
        "week": "15",
        "weekday": "Monday",
        "close": "8375.1346"
    },
    {
        "date": "2017-04-11",
        "month": "04",
        "week": "15",
        "weekday": "Tuesday",
        "close": "8442.2186"
    },
    {
        "date": "2017-04-12",
        "month": "04",
        "week": "15",
        "weekday": "Wednesday",
        "close": "8382.4873"
    },
    {
        "date": "2017-04-13",
        "month": "04",
        "week": "15",
        "weekday": "Thursday",
        "close": "8117.4785"
    },
    {
        "date": "2017-04-14",
        "month": "04",
        "week": "15",
        "weekday": "Friday",
        "close": "8151.1798"
    },
    {
        "date": "2017-04-15",
        "month": "04",
        "week": "15",
        "weekday": "Saturday",
        "close": "8129.5847"
    },
    {
        "date": "2017-04-16",
        "month": "04",
        "week": "15",
        "weekday": "Sunday",
        "close": "8167.0471"
    },
    {
        "date": "2017-04-17",
        "month": "04",
        "week": "16",
        "weekday": "Monday",
        "close": "8267.104"
    },
    {
        "date": "2017-04-18",
        "month": "04",
        "week": "16",
        "weekday": "Tuesday",
        "close": "8379.7232"
    },
    {
        "date": "2017-04-19",
        "month": "04",
        "week": "16",
        "weekday": "Wednesday",
        "close": "8436.3248"
    },
    {
        "date": "2017-04-20",
        "month": "04",
        "week": "16",
        "weekday": "Thursday",
        "close": "8639.4949"
    },
    {
        "date": "2017-04-21",
        "month": "04",
        "week": "16",
        "weekday": "Friday",
        "close": "8654.9971"
    },
    {
        "date": "2017-04-22",
        "month": "04",
        "week": "16",
        "weekday": "Saturday",
        "close": "8567.1483"
    },
    {
        "date": "2017-04-23",
        "month": "04",
        "week": "16",
        "weekday": "Sunday",
        "close": "8458.4188"
    },
    {
        "date": "2017-04-24",
        "month": "04",
        "week": "17",
        "weekday": "Monday",
        "close": "8594.2345"
    },
    {
        "date": "2017-04-25",
        "month": "04",
        "week": "17",
        "weekday": "Tuesday",
        "close": "8700.0125"
    },
    {
        "date": "2017-04-26",
        "month": "04",
        "week": "17",
        "weekday": "Wednesday",
        "close": "8857.1946"
    },
    {
        "date": "2017-04-27",
        "month": "04",
        "week": "17",
        "weekday": "Thursday",
        "close": "9167.2508"
    },
    {
        "date": "2017-04-28",
        "month": "04",
        "week": "17",
        "weekday": "Friday",
        "close": "9101.0934"
    },
    {
        "date": "2017-04-29",
        "month": "04",
        "week": "17",
        "weekday": "Saturday",
        "close": "9149.9325"
    },
    {
        "date": "2017-04-30",
        "month": "04",
        "week": "17",
        "weekday": "Sunday",
        "close": "9325.1119"
    },
    {
        "date": "2017-05-01",
        "month": "05",
        "week": "18",
        "weekday": "Monday",
        "close": "9665.7551"
    },
    {
        "date": "2017-05-02",
        "month": "05",
        "week": "18",
        "weekday": "Tuesday",
        "close": "9944.3653"
    },
    {
        "date": "2017-05-03",
        "month": "05",
        "week": "18",
        "weekday": "Wednesday",
        "close": "10292.3296"
    },
    {
        "date": "2017-05-04",
        "month": "05",
        "week": "18",
        "weekday": "Thursday",
        "close": "10452.0037"
    },
    {
        "date": "2017-05-05",
        "month": "05",
        "week": "18",
        "weekday": "Friday",
        "close": "10439.0799"
    },
    {
        "date": "2017-05-06",
        "month": "05",
        "week": "18",
        "weekday": "Saturday",
        "close": "10688.1301"
    },
    {
        "date": "2017-05-07",
        "month": "05",
        "week": "18",
        "weekday": "Sunday",
        "close": "10660.1939"
    },
    {
        "date": "2017-05-08",
        "month": "05",
        "week": "19",
        "weekday": "Monday",
        "close": "11317.8009"
    },
    {
        "date": "2017-05-09",
        "month": "05",
        "week": "19",
        "weekday": "Tuesday",
        "close": "11794.8949"
    },
    {
        "date": "2017-05-10",
        "month": "05",
        "week": "19",
        "weekday": "Wednesday",
        "close": "12126.2961"
    },
    {
        "date": "2017-05-11",
        "month": "05",
        "week": "19",
        "weekday": "Thursday",
        "close": "12478.0838"
    },
    {
        "date": "2017-05-12",
        "month": "05",
        "week": "19",
        "weekday": "Friday",
        "close": "11569.4125"
    },
    {
        "date": "2017-05-13",
        "month": "05",
        "week": "19",
        "weekday": "Saturday",
        "close": "12141.797"
    },
    {
        "date": "2017-05-14",
        "month": "05",
        "week": "19",
        "weekday": "Sunday",
        "close": "12229.3176"
    },
    {
        "date": "2017-05-15",
        "month": "05",
        "week": "20",
        "weekday": "Monday",
        "close": "11701.2204"
    },
    {
        "date": "2017-05-16",
        "month": "05",
        "week": "20",
        "weekday": "Tuesday",
        "close": "11835.218"
    },
    {
        "date": "2017-05-17",
        "month": "05",
        "week": "20",
        "weekday": "Wednesday",
        "close": "12403.3024"
    },
    {
        "date": "2017-05-18",
        "month": "05",
        "week": "20",
        "weekday": "Thursday",
        "close": "13002.0625"
    },
    {
        "date": "2017-05-19",
        "month": "05",
        "week": "20",
        "weekday": "Friday",
        "close": "13549.3033"
    },
    {
        "date": "2017-05-20",
        "month": "05",
        "week": "20",
        "weekday": "Saturday",
        "close": "14127.3239"
    },
    {
        "date": "2017-05-21",
        "month": "05",
        "week": "20",
        "weekday": "Sunday",
        "close": "14091.8068"
    },
    {
        "date": "2017-05-22",
        "month": "05",
        "week": "21",
        "weekday": "Monday",
        "close": "14731.8028"
    },
    {
        "date": "2017-05-23",
        "month": "05",
        "week": "21",
        "weekday": "Tuesday",
        "close": "15784.8432"
    },
    {
        "date": "2017-05-24",
        "month": "05",
        "week": "21",
        "weekday": "Wednesday",
        "close": "17061.8818"
    },
    {
        "date": "2017-05-25",
        "month": "05",
        "week": "21",
        "weekday": "Thursday",
        "close": "16190.3931"
    },
    {
        "date": "2017-05-26",
        "month": "05",
        "week": "21",
        "weekday": "Friday",
        "close": "15402.2219"
    },
    {
        "date": "2017-05-27",
        "month": "05",
        "week": "21",
        "weekday": "Saturday",
        "close": "14440.0015"
    },
    {
        "date": "2017-05-28",
        "month": "05",
        "week": "21",
        "weekday": "Sunday",
        "close": "15139.4071"
    },
    {
        "date": "2017-05-29",
        "month": "05",
        "week": "22",
        "weekday": "Monday",
        "close": "15700.3794"
    },
    {
        "date": "2017-05-30",
        "month": "05",
        "week": "22",
        "weekday": "Tuesday",
        "close": "15064.5355"
    },
    {
        "date": "2017-05-31",
        "month": "05",
        "week": "22",
        "weekday": "Wednesday",
        "close": "15869.5798"
    },
    {
        "date": "2017-06-01",
        "month": "06",
        "week": "22",
        "weekday": "Thursday",
        "close": "16693.6332"
    },
    {
        "date": "2017-06-02",
        "month": "06",
        "week": "22",
        "weekday": "Friday",
        "close": "17149.9736"
    },
    {
        "date": "2017-06-03",
        "month": "06",
        "week": "22",
        "weekday": "Saturday",
        "close": "17410.0077"
    },
    {
        "date": "2017-06-04",
        "month": "06",
        "week": "22",
        "weekday": "Sunday",
        "close": "17399.0513"
    },
    {
        "date": "2017-06-05",
        "month": "06",
        "week": "23",
        "weekday": "Monday",
        "close": "18621.161"
    },
    {
        "date": "2017-06-06",
        "month": "06",
        "week": "23",
        "weekday": "Tuesday",
        "close": "19797.8391"
    },
    {
        "date": "2017-06-07",
        "month": "06",
        "week": "23",
        "weekday": "Wednesday",
        "close": "18205.3747"
    },
    {
        "date": "2017-06-08",
        "month": "06",
        "week": "23",
        "weekday": "Thursday",
        "close": "19209.0831"
    },
    {
        "date": "2017-06-09",
        "month": "06",
        "week": "23",
        "weekday": "Friday",
        "close": "19218.5925"
    },
    {
        "date": "2017-06-10",
        "month": "06",
        "week": "23",
        "weekday": "Saturday",
        "close": "20004.1207"
    },
    {
        "date": "2017-06-11",
        "month": "06",
        "week": "23",
        "weekday": "Sunday",
        "close": "20472.3611"
    },
    {
        "date": "2017-06-12",
        "month": "06",
        "week": "24",
        "weekday": "Monday",
        "close": "18234.4754"
    },
    {
        "date": "2017-06-13",
        "month": "06",
        "week": "24",
        "weekday": "Tuesday",
        "close": "18615.1877"
    },
    {
        "date": "2017-06-14",
        "month": "06",
        "week": "24",
        "weekday": "Wednesday",
        "close": "16946.0339"
    },
    {
        "date": "2017-06-15",
        "month": "06",
        "week": "24",
        "weekday": "Thursday",
        "close": "16724.4891"
    },
    {
        "date": "2017-06-16",
        "month": "06",
        "week": "24",
        "weekday": "Friday",
        "close": "17217.0095"
    },
    {
        "date": "2017-06-17",
        "month": "06",
        "week": "24",
        "weekday": "Saturday",
        "close": "18142.6219"
    },
    {
        "date": "2017-06-18",
        "month": "06",
        "week": "24",
        "weekday": "Sunday",
        "close": "17535.8535"
    },
    {
        "date": "2017-06-19",
        "month": "06",
        "week": "25",
        "weekday": "Monday",
        "close": "18015.1039"
    },
    {
        "date": "2017-06-20",
        "month": "06",
        "week": "25",
        "weekday": "Tuesday",
        "close": "18975.7796"
    },
    {
        "date": "2017-06-21",
        "month": "06",
        "week": "25",
        "weekday": "Wednesday",
        "close": "18522.6802"
    },
    {
        "date": "2017-06-22",
        "month": "06",
        "week": "25",
        "weekday": "Thursday",
        "close": "18733.2802"
    },
    {
        "date": "2017-06-23",
        "month": "06",
        "week": "25",
        "weekday": "Friday",
        "close": "18720.4229"
    },
    {
        "date": "2017-06-24",
        "month": "06",
        "week": "25",
        "weekday": "Saturday",
        "close": "17906.1295"
    },
    {
        "date": "2017-06-25",
        "month": "06",
        "week": "25",
        "weekday": "Sunday",
        "close": "17734.3884"
    },
    {
        "date": "2017-06-26",
        "month": "06",
        "week": "26",
        "weekday": "Monday",
        "close": "17001.592"
    },
    {
        "date": "2017-06-27",
        "month": "06",
        "week": "26",
        "weekday": "Tuesday",
        "close": "17666.3417"
    },
    {
        "date": "2017-06-28",
        "month": "06",
        "week": "26",
        "weekday": "Wednesday",
        "close": "17575.261"
    },
    {
        "date": "2017-06-29",
        "month": "06",
        "week": "26",
        "weekday": "Thursday",
        "close": "17385.3171"
    },
    {
        "date": "2017-06-30",
        "month": "06",
        "week": "26",
        "weekday": "Friday",
        "close": "16943.0147"
    },
    {
        "date": "2017-07-01",
        "month": "07",
        "week": "26",
        "weekday": "Saturday",
        "close": "16674.129"
    },
    {
        "date": "2017-07-02",
        "month": "07",
        "week": "26",
        "weekday": "Sunday",
        "close": "17150.7103"
    },
    {
        "date": "2017-07-03",
        "month": "07",
        "week": "27",
        "weekday": "Monday",
        "close": "17549.3179"
    },
    {
        "date": "2017-07-04",
        "month": "07",
        "week": "27",
        "weekday": "Tuesday",
        "close": "17851.5456"
    },
    {
        "date": "2017-07-05",
        "month": "07",
        "week": "27",
        "weekday": "Wednesday",
        "close": "17812.6481"
    },
    {
        "date": "2017-07-06",
        "month": "07",
        "week": "27",
        "weekday": "Thursday",
        "close": "17813.1077"
    },
    {
        "date": "2017-07-07",
        "month": "07",
        "week": "27",
        "weekday": "Friday",
        "close": "17156.6351"
    },
    {
        "date": "2017-07-08",
        "month": "07",
        "week": "27",
        "weekday": "Saturday",
        "close": "17557.352"
    },
    {
        "date": "2017-07-09",
        "month": "07",
        "week": "27",
        "weekday": "Sunday",
        "close": "17189.5013"
    },
    {
        "date": "2017-07-10",
        "month": "07",
        "week": "28",
        "weekday": "Monday",
        "close": "16137.2933"
    },
    {
        "date": "2017-07-11",
        "month": "07",
        "week": "28",
        "weekday": "Tuesday",
        "close": "15865.7291"
    },
    {
        "date": "2017-07-12",
        "month": "07",
        "week": "28",
        "weekday": "Wednesday",
        "close": "16446.9487"
    },
    {
        "date": "2017-07-13",
        "month": "07",
        "week": "28",
        "weekday": "Thursday",
        "close": "16036.6222"
    },
    {
        "date": "2017-07-14",
        "month": "07",
        "week": "28",
        "weekday": "Friday",
        "close": "15132.8235"
    },
    {
        "date": "2017-07-15",
        "month": "07",
        "week": "28",
        "weekday": "Saturday",
        "close": "13510.2081"
    },
    {
        "date": "2017-07-16",
        "month": "07",
        "week": "28",
        "weekday": "Sunday",
        "close": "13075.9378"
    },
    {
        "date": "2017-07-17",
        "month": "07",
        "week": "29",
        "weekday": "Monday",
        "close": "15192.6798"
    },
    {
        "date": "2017-07-18",
        "month": "07",
        "week": "29",
        "weekday": "Tuesday",
        "close": "15706.0407"
    },
    {
        "date": "2017-07-19",
        "month": "07",
        "week": "29",
        "weekday": "Wednesday",
        "close": "15491.8987"
    },
    {
        "date": "2017-07-20",
        "month": "07",
        "week": "29",
        "weekday": "Thursday",
        "close": "19449.5488"
    },
    {
        "date": "2017-07-21",
        "month": "07",
        "week": "29",
        "weekday": "Friday",
        "close": "18231.0571"
    },
    {
        "date": "2017-07-22",
        "month": "07",
        "week": "29",
        "weekday": "Saturday",
        "close": "19210.2278"
    },
    {
        "date": "2017-07-23",
        "month": "07",
        "week": "29",
        "weekday": "Sunday",
        "close": "18585.2759"
    },
    {
        "date": "2017-07-24",
        "month": "07",
        "week": "30",
        "weekday": "Monday",
        "close": "18762.6589"
    },
    {
        "date": "2017-07-25",
        "month": "07",
        "week": "30",
        "weekday": "Tuesday",
        "close": "17489.6893"
    },
    {
        "date": "2017-07-26",
        "month": "07",
        "week": "30",
        "weekday": "Wednesday",
        "close": "17219.7355"
    },
    {
        "date": "2017-07-27",
        "month": "07",
        "week": "30",
        "weekday": "Thursday",
        "close": "18188.4669"
    },
    {
        "date": "2017-07-28",
        "month": "07",
        "week": "30",
        "weekday": "Friday",
        "close": "18898.2088"
    },
    {
        "date": "2017-07-29",
        "month": "07",
        "week": "30",
        "weekday": "Saturday",
        "close": "18326.2673"
    },
    {
        "date": "2017-07-30",
        "month": "07",
        "week": "30",
        "weekday": "Sunday",
        "close": "18499.4572"
    },
    {
        "date": "2017-07-31",
        "month": "07",
        "week": "31",
        "weekday": "Monday",
        "close": "19334.0151"
    },
    {
        "date": "2017-08-01",
        "month": "08",
        "week": "31",
        "weekday": "Tuesday",
        "close": "18376.5093"
    },
    {
        "date": "2017-08-02",
        "month": "08",
        "week": "31",
        "weekday": "Wednesday",
        "close": "18305.9985"
    },
    {
        "date": "2017-08-03",
        "month": "08",
        "week": "31",
        "weekday": "Thursday",
        "close": "18901.8131"
    },
    {
        "date": "2017-08-04",
        "month": "08",
        "week": "31",
        "weekday": "Friday",
        "close": "19412.7978"
    },
    {
        "date": "2017-08-05",
        "month": "08",
        "week": "31",
        "weekday": "Saturday",
        "close": "22227.2745"
    },
    {
        "date": "2017-08-06",
        "month": "08",
        "week": "31",
        "weekday": "Sunday",
        "close": "22027.9885"
    },
    {
        "date": "2017-08-07",
        "month": "08",
        "week": "32",
        "weekday": "Monday",
        "close": "23063.1732"
    },
    {
        "date": "2017-08-08",
        "month": "08",
        "week": "32",
        "weekday": "Tuesday",
        "close": "23332.6502"
    },
    {
        "date": "2017-08-09",
        "month": "08",
        "week": "32",
        "weekday": "Wednesday",
        "close": "22541.6557"
    },
    {
        "date": "2017-08-10",
        "month": "08",
        "week": "32",
        "weekday": "Thursday",
        "close": "22904.393"
    },
    {
        "date": "2017-08-11",
        "month": "08",
        "week": "32",
        "weekday": "Friday",
        "close": "24526.2402"
    },
    {
        "date": "2017-08-12",
        "month": "08",
        "week": "32",
        "weekday": "Saturday",
        "close": "26109.954"
    },
    {
        "date": "2017-08-13",
        "month": "08",
        "week": "32",
        "weekday": "Sunday",
        "close": "27390.8412"
    },
    {
        "date": "2017-08-14",
        "month": "08",
        "week": "33",
        "weekday": "Monday",
        "close": "29237.7752"
    },
    {
        "date": "2017-08-15",
        "month": "08",
        "week": "33",
        "weekday": "Tuesday",
        "close": "28073.6691"
    },
    {
        "date": "2017-08-16",
        "month": "08",
        "week": "33",
        "weekday": "Wednesday",
        "close": "29612.7553"
    },
    {
        "date": "2017-08-17",
        "month": "08",
        "week": "33",
        "weekday": "Thursday",
        "close": "28816.4854"
    },
    {
        "date": "2017-08-18",
        "month": "08",
        "week": "33",
        "weekday": "Friday",
        "close": "27752.9395"
    },
    {
        "date": "2017-08-19",
        "month": "08",
        "week": "33",
        "weekday": "Saturday",
        "close": "28062.8866"
    },
    {
        "date": "2017-08-20",
        "month": "08",
        "week": "33",
        "weekday": "Sunday",
        "close": "27416.633"
    },
    {
        "date": "2017-08-21",
        "month": "08",
        "week": "34",
        "weekday": "Monday",
        "close": "26919.0691"
    },
    {
        "date": "2017-08-22",
        "month": "08",
        "week": "34",
        "weekday": "Tuesday",
        "close": "27565.3902"
    },
    {
        "date": "2017-08-23",
        "month": "08",
        "week": "34",
        "weekday": "Wednesday",
        "close": "27907.3345"
    },
    {
        "date": "2017-08-24",
        "month": "08",
        "week": "34",
        "weekday": "Thursday",
        "close": "29063.2438"
    },
    {
        "date": "2017-08-25",
        "month": "08",
        "week": "34",
        "weekday": "Friday",
        "close": "29305.655"
    },
    {
        "date": "2017-08-26",
        "month": "08",
        "week": "34",
        "weekday": "Saturday",
        "close": "29168.7202"
    },
    {
        "date": "2017-08-27",
        "month": "08",
        "week": "34",
        "weekday": "Sunday",
        "close": "28945.352"
    },
    {
        "date": "2017-08-28",
        "month": "08",
        "week": "35",
        "weekday": "Monday",
        "close": "29340.152"
    },
    {
        "date": "2017-08-29",
        "month": "08",
        "week": "35",
        "weekday": "Tuesday",
        "close": "30656.5006"
    },
    {
        "date": "2017-08-30",
        "month": "08",
        "week": "35",
        "weekday": "Wednesday",
        "close": "30532.943"
    },
    {
        "date": "2017-08-31",
        "month": "08",
        "week": "35",
        "weekday": "Thursday",
        "close": "31391.9005"
    },
    {
        "date": "2017-09-01",
        "month": "09",
        "week": "35",
        "weekday": "Friday",
        "close": "32482.9375"
    },
    {
        "date": "2017-09-02",
        "month": "09",
        "week": "35",
        "weekday": "Saturday",
        "close": "30470.2819"
    },
    {
        "date": "2017-09-03",
        "month": "09",
        "week": "35",
        "weekday": "Sunday",
        "close": "30336.7"
    },
    {
        "date": "2017-09-04",
        "month": "09",
        "week": "36",
        "weekday": "Monday",
        "close": "28208.2898"
    },
    {
        "date": "2017-09-05",
        "month": "09",
        "week": "36",
        "weekday": "Tuesday",
        "close": "28918.0205"
    },
    {
        "date": "2017-09-06",
        "month": "09",
        "week": "36",
        "weekday": "Wednesday",
        "close": "30181.6445"
    },
    {
        "date": "2017-09-07",
        "month": "09",
        "week": "36",
        "weekday": "Thursday",
        "close": "30089.0416"
    },
    {
        "date": "2017-09-08",
        "month": "09",
        "week": "36",
        "weekday": "Friday",
        "close": "27976.5645"
    },
    {
        "date": "2017-09-09",
        "month": "09",
        "week": "36",
        "weekday": "Saturday",
        "close": "27818.6782"
    },
    {
        "date": "2017-09-10",
        "month": "09",
        "week": "36",
        "weekday": "Sunday",
        "close": "27359.9317"
    },
    {
        "date": "2017-09-11",
        "month": "09",
        "week": "37",
        "weekday": "Monday",
        "close": "27351.0555"
    },
    {
        "date": "2017-09-12",
        "month": "09",
        "week": "37",
        "weekday": "Tuesday",
        "close": "27111.147"
    },
    {
        "date": "2017-09-13",
        "month": "09",
        "week": "37",
        "weekday": "Wednesday",
        "close": "25354.506"
    },
    {
        "date": "2017-09-14",
        "month": "09",
        "week": "37",
        "weekday": "Thursday",
        "close": "21152.8443"
    },
    {
        "date": "2017-09-15",
        "month": "09",
        "week": "37",
        "weekday": "Friday",
        "close": "24164.8636"
    },
    {
        "date": "2017-09-16",
        "month": "09",
        "week": "37",
        "weekday": "Saturday",
        "close": "24111.3645"
    },
    {
        "date": "2017-09-17",
        "month": "09",
        "week": "37",
        "weekday": "Sunday",
        "close": "24057.8213"
    },
    {
        "date": "2017-09-18",
        "month": "09",
        "week": "38",
        "weekday": "Monday",
        "close": "26737.3742"
    },
    {
        "date": "2017-09-19",
        "month": "09",
        "week": "38",
        "weekday": "Tuesday",
        "close": "25652.4813"
    },
    {
        "date": "2017-09-20",
        "month": "09",
        "week": "38",
        "weekday": "Wednesday",
        "close": "25361.3238"
    },
    {
        "date": "2017-09-21",
        "month": "09",
        "week": "38",
        "weekday": "Thursday",
        "close": "23804.5608"
    },
    {
        "date": "2017-09-22",
        "month": "09",
        "week": "38",
        "weekday": "Friday",
        "close": "23761.1198"
    },
    {
        "date": "2017-09-23",
        "month": "09",
        "week": "38",
        "weekday": "Saturday",
        "close": "24908.4204"
    },
    {
        "date": "2017-09-24",
        "month": "09",
        "week": "38",
        "weekday": "Sunday",
        "close": "24216.5269"
    },
    {
        "date": "2017-09-25",
        "month": "09",
        "week": "39",
        "weekday": "Monday",
        "close": "26007.1112"
    },
    {
        "date": "2017-09-26",
        "month": "09",
        "week": "39",
        "weekday": "Tuesday",
        "close": "25869.3194"
    },
    {
        "date": "2017-09-27",
        "month": "09",
        "week": "39",
        "weekday": "Wednesday",
        "close": "27955.6252"
    },
    {
        "date": "2017-09-28",
        "month": "09",
        "week": "39",
        "weekday": "Thursday",
        "close": "27882.4195"
    },
    {
        "date": "2017-09-29",
        "month": "09",
        "week": "39",
        "weekday": "Friday",
        "close": "27711.6948"
    },
    {
        "date": "2017-09-30",
        "month": "09",
        "week": "39",
        "weekday": "Saturday",
        "close": "28969.0962"
    },
    {
        "date": "2017-10-01",
        "month": "10",
        "week": "39",
        "weekday": "Sunday",
        "close": "29264.4926"
    },
    {
        "date": "2017-10-02",
        "month": "10",
        "week": "40",
        "weekday": "Monday",
        "close": "29295.7562"
    },
    {
        "date": "2017-10-03",
        "month": "10",
        "week": "40",
        "weekday": "Tuesday",
        "close": "28743.0928"
    },
    {
        "date": "2017-10-04",
        "month": "10",
        "week": "40",
        "weekday": "Wednesday",
        "close": "28120.5656"
    },
    {
        "date": "2017-10-05",
        "month": "10",
        "week": "40",
        "weekday": "Thursday",
        "close": "28764.0436"
    },
    {
        "date": "2017-10-06",
        "month": "10",
        "week": "40",
        "weekday": "Friday",
        "close": "29084.1981"
    },
    {
        "date": "2017-10-07",
        "month": "10",
        "week": "40",
        "weekday": "Saturday",
        "close": "29521.3602"
    },
    {
        "date": "2017-10-08",
        "month": "10",
        "week": "40",
        "weekday": "Sunday",
        "close": "30583.2886"
    },
    {
        "date": "2017-10-09",
        "month": "10",
        "week": "41",
        "weekday": "Monday",
        "close": "31622.869"
    },
    {
        "date": "2017-10-10",
        "month": "10",
        "week": "41",
        "weekday": "Tuesday",
        "close": "31243.3645"
    },
    {
        "date": "2017-10-11",
        "month": "10",
        "week": "41",
        "weekday": "Wednesday",
        "close": "31830.4848"
    },
    {
        "date": "2017-10-12",
        "month": "10",
        "week": "41",
        "weekday": "Thursday",
        "close": "35833.2539"
    },
    {
        "date": "2017-10-13",
        "month": "10",
        "week": "41",
        "weekday": "Friday",
        "close": "37106.6814"
    },
    {
        "date": "2017-10-14",
        "month": "10",
        "week": "41",
        "weekday": "Saturday",
        "close": "38222.2666"
    },
    {
        "date": "2017-10-15",
        "month": "10",
        "week": "41",
        "weekday": "Sunday",
        "close": "37517.0856"
    },
    {
        "date": "2017-10-16",
        "month": "10",
        "week": "42",
        "weekday": "Monday",
        "close": "37917.9925"
    },
    {
        "date": "2017-10-17",
        "month": "10",
        "week": "42",
        "weekday": "Tuesday",
        "close": "37060.3182"
    },
    {
        "date": "2017-10-18",
        "month": "10",
        "week": "42",
        "weekday": "Wednesday",
        "close": "36928.632"
    },
    {
        "date": "2017-10-19",
        "month": "10",
        "week": "42",
        "weekday": "Thursday",
        "close": "37704.4579"
    },
    {
        "date": "2017-10-20",
        "month": "10",
        "week": "42",
        "weekday": "Friday",
        "close": "39634.3994"
    },
    {
        "date": "2017-10-21",
        "month": "10",
        "week": "42",
        "weekday": "Saturday",
        "close": "39827.4189"
    },
    {
        "date": "2017-10-22",
        "month": "10",
        "week": "42",
        "weekday": "Sunday",
        "close": "39673.3776"
    },
    {
        "date": "2017-10-23",
        "month": "10",
        "week": "43",
        "weekday": "Monday",
        "close": "39144.7834"
    },
    {
        "date": "2017-10-24",
        "month": "10",
        "week": "43",
        "weekday": "Tuesday",
        "close": "36611.829"
    },
    {
        "date": "2017-10-25",
        "month": "10",
        "week": "43",
        "weekday": "Wednesday",
        "close": "38067.9613"
    },
    {
        "date": "2017-10-26",
        "month": "10",
        "week": "43",
        "weekday": "Thursday",
        "close": "39108.6475"
    },
    {
        "date": "2017-10-27",
        "month": "10",
        "week": "43",
        "weekday": "Friday",
        "close": "38353.9173"
    },
    {
        "date": "2017-10-28",
        "month": "10",
        "week": "43",
        "weekday": "Saturday",
        "close": "38122.1385"
    },
    {
        "date": "2017-10-29",
        "month": "10",
        "week": "43",
        "weekday": "Sunday",
        "close": "40925.4142"
    },
    {
        "date": "2017-10-30",
        "month": "10",
        "week": "44",
        "weekday": "Monday",
        "close": "40682.7268"
    },
    {
        "date": "2017-10-31",
        "month": "10",
        "week": "44",
        "weekday": "Tuesday",
        "close": "42779.3067"
    },
    {
        "date": "2017-11-01",
        "month": "11",
        "week": "44",
        "weekday": "Wednesday",
        "close": "44572.0627"
    },
    {
        "date": "2017-11-02",
        "month": "11",
        "week": "44",
        "weekday": "Thursday",
        "close": "46462.0654"
    },
    {
        "date": "2017-11-03",
        "month": "11",
        "week": "44",
        "weekday": "Friday",
        "close": "47518.0205"
    },
    {
        "date": "2017-11-04",
        "month": "11",
        "week": "44",
        "weekday": "Saturday",
        "close": "49047.8253"
    },
    {
        "date": "2017-11-05",
        "month": "11",
        "week": "44",
        "weekday": "Sunday",
        "close": "48907.9843"
    },
    {
        "date": "2017-11-06",
        "month": "11",
        "week": "45",
        "weekday": "Monday",
        "close": "46159.7307"
    },
    {
        "date": "2017-11-07",
        "month": "11",
        "week": "45",
        "weekday": "Tuesday",
        "close": "47249.3415"
    },
    {
        "date": "2017-11-08",
        "month": "11",
        "week": "45",
        "weekday": "Wednesday",
        "close": "49427.7048"
    },
    {
        "date": "2017-11-09",
        "month": "11",
        "week": "45",
        "weekday": "Thursday",
        "close": "47448.9118"
    },
    {
        "date": "2017-11-10",
        "month": "11",
        "week": "45",
        "weekday": "Friday",
        "close": "43637.0596"
    },
    {
        "date": "2017-11-11",
        "month": "11",
        "week": "45",
        "weekday": "Saturday",
        "close": "42085.6019"
    },
    {
        "date": "2017-11-12",
        "month": "11",
        "week": "45",
        "weekday": "Sunday",
        "close": "38904.304"
    },
    {
        "date": "2017-11-13",
        "month": "11",
        "week": "46",
        "weekday": "Monday",
        "close": "43279.9771"
    },
    {
        "date": "2017-11-14",
        "month": "11",
        "week": "46",
        "weekday": "Tuesday",
        "close": "43801.9668"
    },
    {
        "date": "2017-11-15",
        "month": "11",
        "week": "46",
        "weekday": "Wednesday",
        "close": "48232.4671"
    },
    {
        "date": "2017-11-16",
        "month": "11",
        "week": "46",
        "weekday": "Thursday",
        "close": "52012.2859"
    },
    {
        "date": "2017-11-17",
        "month": "11",
        "week": "46",
        "weekday": "Friday",
        "close": "50973.7739"
    },
    {
        "date": "2017-11-18",
        "month": "11",
        "week": "46",
        "weekday": "Saturday",
        "close": "51542.0595"
    },
    {
        "date": "2017-11-19",
        "month": "11",
        "week": "46",
        "weekday": "Sunday",
        "close": "53279.4604"
    },
    {
        "date": "2017-11-20",
        "month": "11",
        "week": "47",
        "weekday": "Monday",
        "close": "54656.3545"
    },
    {
        "date": "2017-11-21",
        "month": "11",
        "week": "47",
        "weekday": "Tuesday",
        "close": "53673.7877"
    },
    {
        "date": "2017-11-22",
        "month": "11",
        "week": "47",
        "weekday": "Wednesday",
        "close": "54403.2305"
    },
    {
        "date": "2017-11-23",
        "month": "11",
        "week": "47",
        "weekday": "Thursday",
        "close": "52676.5845"
    },
    {
        "date": "2017-11-24",
        "month": "11",
        "week": "47",
        "weekday": "Friday",
        "close": "54136.6166"
    },
    {
        "date": "2017-11-25",
        "month": "11",
        "week": "47",
        "weekday": "Saturday",
        "close": "57851.0594"
    },
    {
        "date": "2017-11-26",
        "month": "11",
        "week": "47",
        "weekday": "Sunday",
        "close": "60980.0178"
    },
    {
        "date": "2017-11-27",
        "month": "11",
        "week": "48",
        "weekday": "Monday",
        "close": "64246.5971"
    },
    {
        "date": "2017-11-28",
        "month": "11",
        "week": "48",
        "weekday": "Tuesday",
        "close": "65458.8612"
    },
    {
        "date": "2017-11-29",
        "month": "11",
        "week": "48",
        "weekday": "Wednesday",
        "close": "64890.9642"
    },
    {
        "date": "2017-11-30",
        "month": "11",
        "week": "48",
        "weekday": "Thursday",
        "close": "65583.2597"
    },
    {
        "date": "2017-12-01",
        "month": "12",
        "week": "48",
        "weekday": "Friday",
        "close": "71825.6883"
    },
    {
        "date": "2017-12-02",
        "month": "12",
        "week": "48",
        "weekday": "Saturday",
        "close": "72079.2312"
    },
    {
        "date": "2017-12-03",
        "month": "12",
        "week": "48",
        "weekday": "Sunday",
        "close": "74007.4136"
    },
    {
        "date": "2017-12-04",
        "month": "12",
        "week": "49",
        "weekday": "Monday",
        "close": "76852.0129"
    },
    {
        "date": "2017-12-05",
        "month": "12",
        "week": "49",
        "weekday": "Tuesday",
        "close": "77398.8752"
    },
    {
        "date": "2017-12-06",
        "month": "12",
        "week": "49",
        "weekday": "Wednesday",
        "close": "90679.5487"
    },
    {
        "date": "2017-12-07",
        "month": "12",
        "week": "49",
        "weekday": "Thursday",
        "close": "111589.9776"
    },
    {
        "date": "2017-12-08",
        "month": "12",
        "week": "49",
        "weekday": "Friday",
        "close": "106233.201"
    },
    {
        "date": "2017-12-09",
        "month": "12",
        "week": "49",
        "weekday": "Saturday",
        "close": "98676.7747"
    },
    {
        "date": "2017-12-10",
        "month": "12",
        "week": "49",
        "weekday": "Sunday",
        "close": "99525.1027"
    },
    {
        "date": "2017-12-11",
        "month": "12",
        "week": "50",
        "weekday": "Monday",
        "close": "110642.88"
    },
    {
        "date": "2017-12-12",
        "month": "12",
        "week": "50",
        "weekday": "Tuesday",
        "close": "113732.6745"
    }]
    


```python
# 第二种方法，用 requests.get 来从网上下载
import requests
json_url = 'https://raw.githubusercontent.com/muxuezi/btc/master/btc_close_2017.json'
req = requests.get(json_url)
# 写入文件
with open('btc_close_2017_request.json','w') as f:
    f.write(req.text)
file_requests = req.json()
print(file_urllib == file_requests)
```

    True
    


```python
print(req.text)
```

    [{
        "date": "2017-01-01",
        "month": "01",
        "week": "52",
        "weekday": "Sunday",
        "close": "6928.6492"
    },
    {
        "date": "2017-01-02",
        "month": "01",
        "week": "1",
        "weekday": "Monday",
        "close": "7070.2554"
    },
    {
        "date": "2017-01-03",
        "month": "01",
        "week": "1",
        "weekday": "Tuesday",
        "close": "7175.1082"
    },
    {
        "date": "2017-01-04",
        "month": "01",
        "week": "1",
        "weekday": "Wednesday",
        "close": "7835.7615"
    },
    {
        "date": "2017-01-05",
        "month": "01",
        "week": "1",
        "weekday": "Thursday",
        "close": "6928.7578"
    },
    {
        "date": "2017-01-06",
        "month": "01",
        "week": "1",
        "weekday": "Friday",
        "close": "6196.6928"
    },
    {
        "date": "2017-01-07",
        "month": "01",
        "week": "1",
        "weekday": "Saturday",
        "close": "6262.1471"
    },
    {
        "date": "2017-01-08",
        "month": "01",
        "week": "1",
        "weekday": "Sunday",
        "close": "6319.9404"
    },
    {
        "date": "2017-01-09",
        "month": "01",
        "week": "2",
        "weekday": "Monday",
        "close": "6239.1506"
    },
    {
        "date": "2017-01-10",
        "month": "01",
        "week": "2",
        "weekday": "Tuesday",
        "close": "6263.1548"
    },
    {
        "date": "2017-01-11",
        "month": "01",
        "week": "2",
        "weekday": "Wednesday",
        "close": "5383.0598"
    },
    {
        "date": "2017-01-12",
        "month": "01",
        "week": "2",
        "weekday": "Thursday",
        "close": "5566.7345"
    },
    {
        "date": "2017-01-13",
        "month": "01",
        "week": "2",
        "weekday": "Friday",
        "close": "5700.0716"
    },
    {
        "date": "2017-01-14",
        "month": "01",
        "week": "2",
        "weekday": "Saturday",
        "close": "5648.6897"
    },
    {
        "date": "2017-01-15",
        "month": "01",
        "week": "2",
        "weekday": "Sunday",
        "close": "5674.7977"
    },
    {
        "date": "2017-01-16",
        "month": "01",
        "week": "3",
        "weekday": "Monday",
        "close": "5730.0658"
    },
    {
        "date": "2017-01-17",
        "month": "01",
        "week": "3",
        "weekday": "Tuesday",
        "close": "6202.9704"
    },
    {
        "date": "2017-01-18",
        "month": "01",
        "week": "3",
        "weekday": "Wednesday",
        "close": "6047.6601"
    },
    {
        "date": "2017-01-19",
        "month": "01",
        "week": "3",
        "weekday": "Thursday",
        "close": "6170.8433"
    },
    {
        "date": "2017-01-20",
        "month": "01",
        "week": "3",
        "weekday": "Friday",
        "close": "6131.2511"
    },
    {
        "date": "2017-01-21",
        "month": "01",
        "week": "3",
        "weekday": "Saturday",
        "close": "6326.3657"
    },
    {
        "date": "2017-01-22",
        "month": "01",
        "week": "3",
        "weekday": "Sunday",
        "close": "6362.9482"
    },
    {
        "date": "2017-01-23",
        "month": "01",
        "week": "4",
        "weekday": "Monday",
        "close": "6255.5602"
    },
    {
        "date": "2017-01-24",
        "month": "01",
        "week": "4",
        "weekday": "Tuesday",
        "close": "6074.8333"
    },
    {
        "date": "2017-01-25",
        "month": "01",
        "week": "4",
        "weekday": "Wednesday",
        "close": "6154.6958"
    },
    {
        "date": "2017-01-26",
        "month": "01",
        "week": "4",
        "weekday": "Thursday",
        "close": "6295.3388"
    },
    {
        "date": "2017-01-27",
        "month": "01",
        "week": "4",
        "weekday": "Friday",
        "close": "6320.7206"
    },
    {
        "date": "2017-01-28",
        "month": "01",
        "week": "4",
        "weekday": "Saturday",
        "close": "6332.5389"
    },
    {
        "date": "2017-01-29",
        "month": "01",
        "week": "4",
        "weekday": "Sunday",
        "close": "6289.1698"
    },
    {
        "date": "2017-01-30",
        "month": "01",
        "week": "5",
        "weekday": "Monday",
        "close": "6332.8246"
    },
    {
        "date": "2017-01-31",
        "month": "01",
        "week": "5",
        "weekday": "Tuesday",
        "close": "6657.8667"
    },
    {
        "date": "2017-02-01",
        "month": "02",
        "week": "5",
        "weekday": "Wednesday",
        "close": "6793.7077"
    },
    {
        "date": "2017-02-02",
        "month": "02",
        "week": "5",
        "weekday": "Thursday",
        "close": "6934.3856"
    },
    {
        "date": "2017-02-03",
        "month": "02",
        "week": "5",
        "weekday": "Friday",
        "close": "6995.2901"
    },
    {
        "date": "2017-02-04",
        "month": "02",
        "week": "5",
        "weekday": "Saturday",
        "close": "7102.0714"
    },
    {
        "date": "2017-02-05",
        "month": "02",
        "week": "5",
        "weekday": "Sunday",
        "close": "6965.9773"
    },
    {
        "date": "2017-02-06",
        "month": "02",
        "week": "6",
        "weekday": "Monday",
        "close": "7034.2211"
    },
    {
        "date": "2017-02-07",
        "month": "02",
        "week": "6",
        "weekday": "Tuesday",
        "close": "7245.8877"
    },
    {
        "date": "2017-02-08",
        "month": "02",
        "week": "6",
        "weekday": "Wednesday",
        "close": "7246.6303"
    },
    {
        "date": "2017-02-09",
        "month": "02",
        "week": "6",
        "weekday": "Thursday",
        "close": "6811.6794"
    },
    {
        "date": "2017-02-10",
        "month": "02",
        "week": "6",
        "weekday": "Friday",
        "close": "6833.4884"
    },
    {
        "date": "2017-02-11",
        "month": "02",
        "week": "6",
        "weekday": "Saturday",
        "close": "6946.09"
    },
    {
        "date": "2017-02-12",
        "month": "02",
        "week": "6",
        "weekday": "Sunday",
        "close": "6883.9424"
    },
    {
        "date": "2017-02-13",
        "month": "02",
        "week": "7",
        "weekday": "Monday",
        "close": "6858.5789"
    },
    {
        "date": "2017-02-14",
        "month": "02",
        "week": "7",
        "weekday": "Tuesday",
        "close": "6930.882"
    },
    {
        "date": "2017-02-15",
        "month": "02",
        "week": "7",
        "weekday": "Wednesday",
        "close": "6935.3788"
    },
    {
        "date": "2017-02-16",
        "month": "02",
        "week": "7",
        "weekday": "Thursday",
        "close": "7088.8535"
    },
    {
        "date": "2017-02-17",
        "month": "02",
        "week": "7",
        "weekday": "Friday",
        "close": "7229.5808"
    },
    {
        "date": "2017-02-18",
        "month": "02",
        "week": "7",
        "weekday": "Saturday",
        "close": "7267.5468"
    },
    {
        "date": "2017-02-19",
        "month": "02",
        "week": "7",
        "weekday": "Sunday",
        "close": "7220.5385"
    },
    {
        "date": "2017-02-20",
        "month": "02",
        "week": "8",
        "weekday": "Monday",
        "close": "7450.2901"
    },
    {
        "date": "2017-02-21",
        "month": "02",
        "week": "8",
        "weekday": "Tuesday",
        "close": "7732.4979"
    },
    {
        "date": "2017-02-22",
        "month": "02",
        "week": "8",
        "weekday": "Wednesday",
        "close": "7716.2218"
    },
    {
        "date": "2017-02-23",
        "month": "02",
        "week": "8",
        "weekday": "Thursday",
        "close": "8092.0221"
    },
    {
        "date": "2017-02-24",
        "month": "02",
        "week": "8",
        "weekday": "Friday",
        "close": "8109.1867"
    },
    {
        "date": "2017-02-25",
        "month": "02",
        "week": "8",
        "weekday": "Saturday",
        "close": "7908.54"
    },
    {
        "date": "2017-02-26",
        "month": "02",
        "week": "8",
        "weekday": "Sunday",
        "close": "8137.4131"
    },
    {
        "date": "2017-02-27",
        "month": "02",
        "week": "9",
        "weekday": "Monday",
        "close": "8206.1829"
    },
    {
        "date": "2017-02-28",
        "month": "02",
        "week": "9",
        "weekday": "Tuesday",
        "close": "8176.3692"
    },
    {
        "date": "2017-03-01",
        "month": "03",
        "week": "9",
        "weekday": "Wednesday",
        "close": "8464.3549"
    },
    {
        "date": "2017-03-02",
        "month": "03",
        "week": "9",
        "weekday": "Thursday",
        "close": "8688.7751"
    },
    {
        "date": "2017-03-03",
        "month": "03",
        "week": "9",
        "weekday": "Friday",
        "close": "8900.4858"
    },
    {
        "date": "2017-03-04",
        "month": "03",
        "week": "9",
        "weekday": "Saturday",
        "close": "8741.0338"
    },
    {
        "date": "2017-03-05",
        "month": "03",
        "week": "9",
        "weekday": "Sunday",
        "close": "8816.6651"
    },
    {
        "date": "2017-03-06",
        "month": "03",
        "week": "10",
        "weekday": "Monday",
        "close": "8832.9615"
    },
    {
        "date": "2017-03-07",
        "month": "03",
        "week": "10",
        "weekday": "Tuesday",
        "close": "8504.1113"
    },
    {
        "date": "2017-03-08",
        "month": "03",
        "week": "10",
        "weekday": "Wednesday",
        "close": "7953.9243"
    },
    {
        "date": "2017-03-09",
        "month": "03",
        "week": "10",
        "weekday": "Thursday",
        "close": "8235.799"
    },
    {
        "date": "2017-03-10",
        "month": "03",
        "week": "10",
        "weekday": "Friday",
        "close": "7716.1296"
    },
    {
        "date": "2017-03-11",
        "month": "03",
        "week": "10",
        "weekday": "Saturday",
        "close": "8161.7404"
    },
    {
        "date": "2017-03-12",
        "month": "03",
        "week": "10",
        "weekday": "Sunday",
        "close": "8441.5353"
    },
    {
        "date": "2017-03-13",
        "month": "03",
        "week": "11",
        "weekday": "Monday",
        "close": "8595.5263"
    },
    {
        "date": "2017-03-14",
        "month": "03",
        "week": "11",
        "weekday": "Tuesday",
        "close": "8616.949"
    },
    {
        "date": "2017-03-15",
        "month": "03",
        "week": "11",
        "weekday": "Wednesday",
        "close": "8711.306"
    },
    {
        "date": "2017-03-16",
        "month": "03",
        "week": "11",
        "weekday": "Thursday",
        "close": "8091.7411"
    },
    {
        "date": "2017-03-17",
        "month": "03",
        "week": "11",
        "weekday": "Friday",
        "close": "7379.6562"
    },
    {
        "date": "2017-03-18",
        "month": "03",
        "week": "11",
        "weekday": "Saturday",
        "close": "6694.36"
    },
    {
        "date": "2017-03-19",
        "month": "03",
        "week": "11",
        "weekday": "Sunday",
        "close": "7028.0107"
    },
    {
        "date": "2017-03-20",
        "month": "03",
        "week": "12",
        "weekday": "Monday",
        "close": "7196.3568"
    },
    {
        "date": "2017-03-21",
        "month": "03",
        "week": "12",
        "weekday": "Tuesday",
        "close": "7680.723"
    },
    {
        "date": "2017-03-22",
        "month": "03",
        "week": "12",
        "weekday": "Wednesday",
        "close": "7139.7016"
    },
    {
        "date": "2017-03-23",
        "month": "03",
        "week": "12",
        "weekday": "Thursday",
        "close": "7092.2246"
    },
    {
        "date": "2017-03-24",
        "month": "03",
        "week": "12",
        "weekday": "Friday",
        "close": "6437.3431"
    },
    {
        "date": "2017-03-25",
        "month": "03",
        "week": "12",
        "weekday": "Saturday",
        "close": "6640.554"
    },
    {
        "date": "2017-03-26",
        "month": "03",
        "week": "12",
        "weekday": "Sunday",
        "close": "6623.5896"
    },
    {
        "date": "2017-03-27",
        "month": "03",
        "week": "13",
        "weekday": "Monday",
        "close": "7151.8202"
    },
    {
        "date": "2017-03-28",
        "month": "03",
        "week": "13",
        "weekday": "Tuesday",
        "close": "7184.6725"
    },
    {
        "date": "2017-03-29",
        "month": "03",
        "week": "13",
        "weekday": "Wednesday",
        "close": "7168.8792"
    },
    {
        "date": "2017-03-30",
        "month": "03",
        "week": "13",
        "weekday": "Thursday",
        "close": "7146.3119"
    },
    {
        "date": "2017-03-31",
        "month": "03",
        "week": "13",
        "weekday": "Friday",
        "close": "7439.1397"
    },
    {
        "date": "2017-04-01",
        "month": "04",
        "week": "13",
        "weekday": "Saturday",
        "close": "7506.4038"
    },
    {
        "date": "2017-04-02",
        "month": "04",
        "week": "13",
        "weekday": "Sunday",
        "close": "7566.0156"
    },
    {
        "date": "2017-04-03",
        "month": "04",
        "week": "14",
        "weekday": "Monday",
        "close": "7903.6773"
    },
    {
        "date": "2017-04-04",
        "month": "04",
        "week": "14",
        "weekday": "Tuesday",
        "close": "7874.9773"
    },
    {
        "date": "2017-04-05",
        "month": "04",
        "week": "14",
        "weekday": "Wednesday",
        "close": "7827.8202"
    },
    {
        "date": "2017-04-06",
        "month": "04",
        "week": "14",
        "weekday": "Thursday",
        "close": "8212.9762"
    },
    {
        "date": "2017-04-07",
        "month": "04",
        "week": "14",
        "weekday": "Friday",
        "close": "8236.9016"
    },
    {
        "date": "2017-04-08",
        "month": "04",
        "week": "14",
        "weekday": "Saturday",
        "close": "8180.3212"
    },
    {
        "date": "2017-04-09",
        "month": "04",
        "week": "14",
        "weekday": "Sunday",
        "close": "8354.8293"
    },
    {
        "date": "2017-04-10",
        "month": "04",
        "week": "15",
        "weekday": "Monday",
        "close": "8375.1346"
    },
    {
        "date": "2017-04-11",
        "month": "04",
        "week": "15",
        "weekday": "Tuesday",
        "close": "8442.2186"
    },
    {
        "date": "2017-04-12",
        "month": "04",
        "week": "15",
        "weekday": "Wednesday",
        "close": "8382.4873"
    },
    {
        "date": "2017-04-13",
        "month": "04",
        "week": "15",
        "weekday": "Thursday",
        "close": "8117.4785"
    },
    {
        "date": "2017-04-14",
        "month": "04",
        "week": "15",
        "weekday": "Friday",
        "close": "8151.1798"
    },
    {
        "date": "2017-04-15",
        "month": "04",
        "week": "15",
        "weekday": "Saturday",
        "close": "8129.5847"
    },
    {
        "date": "2017-04-16",
        "month": "04",
        "week": "15",
        "weekday": "Sunday",
        "close": "8167.0471"
    },
    {
        "date": "2017-04-17",
        "month": "04",
        "week": "16",
        "weekday": "Monday",
        "close": "8267.104"
    },
    {
        "date": "2017-04-18",
        "month": "04",
        "week": "16",
        "weekday": "Tuesday",
        "close": "8379.7232"
    },
    {
        "date": "2017-04-19",
        "month": "04",
        "week": "16",
        "weekday": "Wednesday",
        "close": "8436.3248"
    },
    {
        "date": "2017-04-20",
        "month": "04",
        "week": "16",
        "weekday": "Thursday",
        "close": "8639.4949"
    },
    {
        "date": "2017-04-21",
        "month": "04",
        "week": "16",
        "weekday": "Friday",
        "close": "8654.9971"
    },
    {
        "date": "2017-04-22",
        "month": "04",
        "week": "16",
        "weekday": "Saturday",
        "close": "8567.1483"
    },
    {
        "date": "2017-04-23",
        "month": "04",
        "week": "16",
        "weekday": "Sunday",
        "close": "8458.4188"
    },
    {
        "date": "2017-04-24",
        "month": "04",
        "week": "17",
        "weekday": "Monday",
        "close": "8594.2345"
    },
    {
        "date": "2017-04-25",
        "month": "04",
        "week": "17",
        "weekday": "Tuesday",
        "close": "8700.0125"
    },
    {
        "date": "2017-04-26",
        "month": "04",
        "week": "17",
        "weekday": "Wednesday",
        "close": "8857.1946"
    },
    {
        "date": "2017-04-27",
        "month": "04",
        "week": "17",
        "weekday": "Thursday",
        "close": "9167.2508"
    },
    {
        "date": "2017-04-28",
        "month": "04",
        "week": "17",
        "weekday": "Friday",
        "close": "9101.0934"
    },
    {
        "date": "2017-04-29",
        "month": "04",
        "week": "17",
        "weekday": "Saturday",
        "close": "9149.9325"
    },
    {
        "date": "2017-04-30",
        "month": "04",
        "week": "17",
        "weekday": "Sunday",
        "close": "9325.1119"
    },
    {
        "date": "2017-05-01",
        "month": "05",
        "week": "18",
        "weekday": "Monday",
        "close": "9665.7551"
    },
    {
        "date": "2017-05-02",
        "month": "05",
        "week": "18",
        "weekday": "Tuesday",
        "close": "9944.3653"
    },
    {
        "date": "2017-05-03",
        "month": "05",
        "week": "18",
        "weekday": "Wednesday",
        "close": "10292.3296"
    },
    {
        "date": "2017-05-04",
        "month": "05",
        "week": "18",
        "weekday": "Thursday",
        "close": "10452.0037"
    },
    {
        "date": "2017-05-05",
        "month": "05",
        "week": "18",
        "weekday": "Friday",
        "close": "10439.0799"
    },
    {
        "date": "2017-05-06",
        "month": "05",
        "week": "18",
        "weekday": "Saturday",
        "close": "10688.1301"
    },
    {
        "date": "2017-05-07",
        "month": "05",
        "week": "18",
        "weekday": "Sunday",
        "close": "10660.1939"
    },
    {
        "date": "2017-05-08",
        "month": "05",
        "week": "19",
        "weekday": "Monday",
        "close": "11317.8009"
    },
    {
        "date": "2017-05-09",
        "month": "05",
        "week": "19",
        "weekday": "Tuesday",
        "close": "11794.8949"
    },
    {
        "date": "2017-05-10",
        "month": "05",
        "week": "19",
        "weekday": "Wednesday",
        "close": "12126.2961"
    },
    {
        "date": "2017-05-11",
        "month": "05",
        "week": "19",
        "weekday": "Thursday",
        "close": "12478.0838"
    },
    {
        "date": "2017-05-12",
        "month": "05",
        "week": "19",
        "weekday": "Friday",
        "close": "11569.4125"
    },
    {
        "date": "2017-05-13",
        "month": "05",
        "week": "19",
        "weekday": "Saturday",
        "close": "12141.797"
    },
    {
        "date": "2017-05-14",
        "month": "05",
        "week": "19",
        "weekday": "Sunday",
        "close": "12229.3176"
    },
    {
        "date": "2017-05-15",
        "month": "05",
        "week": "20",
        "weekday": "Monday",
        "close": "11701.2204"
    },
    {
        "date": "2017-05-16",
        "month": "05",
        "week": "20",
        "weekday": "Tuesday",
        "close": "11835.218"
    },
    {
        "date": "2017-05-17",
        "month": "05",
        "week": "20",
        "weekday": "Wednesday",
        "close": "12403.3024"
    },
    {
        "date": "2017-05-18",
        "month": "05",
        "week": "20",
        "weekday": "Thursday",
        "close": "13002.0625"
    },
    {
        "date": "2017-05-19",
        "month": "05",
        "week": "20",
        "weekday": "Friday",
        "close": "13549.3033"
    },
    {
        "date": "2017-05-20",
        "month": "05",
        "week": "20",
        "weekday": "Saturday",
        "close": "14127.3239"
    },
    {
        "date": "2017-05-21",
        "month": "05",
        "week": "20",
        "weekday": "Sunday",
        "close": "14091.8068"
    },
    {
        "date": "2017-05-22",
        "month": "05",
        "week": "21",
        "weekday": "Monday",
        "close": "14731.8028"
    },
    {
        "date": "2017-05-23",
        "month": "05",
        "week": "21",
        "weekday": "Tuesday",
        "close": "15784.8432"
    },
    {
        "date": "2017-05-24",
        "month": "05",
        "week": "21",
        "weekday": "Wednesday",
        "close": "17061.8818"
    },
    {
        "date": "2017-05-25",
        "month": "05",
        "week": "21",
        "weekday": "Thursday",
        "close": "16190.3931"
    },
    {
        "date": "2017-05-26",
        "month": "05",
        "week": "21",
        "weekday": "Friday",
        "close": "15402.2219"
    },
    {
        "date": "2017-05-27",
        "month": "05",
        "week": "21",
        "weekday": "Saturday",
        "close": "14440.0015"
    },
    {
        "date": "2017-05-28",
        "month": "05",
        "week": "21",
        "weekday": "Sunday",
        "close": "15139.4071"
    },
    {
        "date": "2017-05-29",
        "month": "05",
        "week": "22",
        "weekday": "Monday",
        "close": "15700.3794"
    },
    {
        "date": "2017-05-30",
        "month": "05",
        "week": "22",
        "weekday": "Tuesday",
        "close": "15064.5355"
    },
    {
        "date": "2017-05-31",
        "month": "05",
        "week": "22",
        "weekday": "Wednesday",
        "close": "15869.5798"
    },
    {
        "date": "2017-06-01",
        "month": "06",
        "week": "22",
        "weekday": "Thursday",
        "close": "16693.6332"
    },
    {
        "date": "2017-06-02",
        "month": "06",
        "week": "22",
        "weekday": "Friday",
        "close": "17149.9736"
    },
    {
        "date": "2017-06-03",
        "month": "06",
        "week": "22",
        "weekday": "Saturday",
        "close": "17410.0077"
    },
    {
        "date": "2017-06-04",
        "month": "06",
        "week": "22",
        "weekday": "Sunday",
        "close": "17399.0513"
    },
    {
        "date": "2017-06-05",
        "month": "06",
        "week": "23",
        "weekday": "Monday",
        "close": "18621.161"
    },
    {
        "date": "2017-06-06",
        "month": "06",
        "week": "23",
        "weekday": "Tuesday",
        "close": "19797.8391"
    },
    {
        "date": "2017-06-07",
        "month": "06",
        "week": "23",
        "weekday": "Wednesday",
        "close": "18205.3747"
    },
    {
        "date": "2017-06-08",
        "month": "06",
        "week": "23",
        "weekday": "Thursday",
        "close": "19209.0831"
    },
    {
        "date": "2017-06-09",
        "month": "06",
        "week": "23",
        "weekday": "Friday",
        "close": "19218.5925"
    },
    {
        "date": "2017-06-10",
        "month": "06",
        "week": "23",
        "weekday": "Saturday",
        "close": "20004.1207"
    },
    {
        "date": "2017-06-11",
        "month": "06",
        "week": "23",
        "weekday": "Sunday",
        "close": "20472.3611"
    },
    {
        "date": "2017-06-12",
        "month": "06",
        "week": "24",
        "weekday": "Monday",
        "close": "18234.4754"
    },
    {
        "date": "2017-06-13",
        "month": "06",
        "week": "24",
        "weekday": "Tuesday",
        "close": "18615.1877"
    },
    {
        "date": "2017-06-14",
        "month": "06",
        "week": "24",
        "weekday": "Wednesday",
        "close": "16946.0339"
    },
    {
        "date": "2017-06-15",
        "month": "06",
        "week": "24",
        "weekday": "Thursday",
        "close": "16724.4891"
    },
    {
        "date": "2017-06-16",
        "month": "06",
        "week": "24",
        "weekday": "Friday",
        "close": "17217.0095"
    },
    {
        "date": "2017-06-17",
        "month": "06",
        "week": "24",
        "weekday": "Saturday",
        "close": "18142.6219"
    },
    {
        "date": "2017-06-18",
        "month": "06",
        "week": "24",
        "weekday": "Sunday",
        "close": "17535.8535"
    },
    {
        "date": "2017-06-19",
        "month": "06",
        "week": "25",
        "weekday": "Monday",
        "close": "18015.1039"
    },
    {
        "date": "2017-06-20",
        "month": "06",
        "week": "25",
        "weekday": "Tuesday",
        "close": "18975.7796"
    },
    {
        "date": "2017-06-21",
        "month": "06",
        "week": "25",
        "weekday": "Wednesday",
        "close": "18522.6802"
    },
    {
        "date": "2017-06-22",
        "month": "06",
        "week": "25",
        "weekday": "Thursday",
        "close": "18733.2802"
    },
    {
        "date": "2017-06-23",
        "month": "06",
        "week": "25",
        "weekday": "Friday",
        "close": "18720.4229"
    },
    {
        "date": "2017-06-24",
        "month": "06",
        "week": "25",
        "weekday": "Saturday",
        "close": "17906.1295"
    },
    {
        "date": "2017-06-25",
        "month": "06",
        "week": "25",
        "weekday": "Sunday",
        "close": "17734.3884"
    },
    {
        "date": "2017-06-26",
        "month": "06",
        "week": "26",
        "weekday": "Monday",
        "close": "17001.592"
    },
    {
        "date": "2017-06-27",
        "month": "06",
        "week": "26",
        "weekday": "Tuesday",
        "close": "17666.3417"
    },
    {
        "date": "2017-06-28",
        "month": "06",
        "week": "26",
        "weekday": "Wednesday",
        "close": "17575.261"
    },
    {
        "date": "2017-06-29",
        "month": "06",
        "week": "26",
        "weekday": "Thursday",
        "close": "17385.3171"
    },
    {
        "date": "2017-06-30",
        "month": "06",
        "week": "26",
        "weekday": "Friday",
        "close": "16943.0147"
    },
    {
        "date": "2017-07-01",
        "month": "07",
        "week": "26",
        "weekday": "Saturday",
        "close": "16674.129"
    },
    {
        "date": "2017-07-02",
        "month": "07",
        "week": "26",
        "weekday": "Sunday",
        "close": "17150.7103"
    },
    {
        "date": "2017-07-03",
        "month": "07",
        "week": "27",
        "weekday": "Monday",
        "close": "17549.3179"
    },
    {
        "date": "2017-07-04",
        "month": "07",
        "week": "27",
        "weekday": "Tuesday",
        "close": "17851.5456"
    },
    {
        "date": "2017-07-05",
        "month": "07",
        "week": "27",
        "weekday": "Wednesday",
        "close": "17812.6481"
    },
    {
        "date": "2017-07-06",
        "month": "07",
        "week": "27",
        "weekday": "Thursday",
        "close": "17813.1077"
    },
    {
        "date": "2017-07-07",
        "month": "07",
        "week": "27",
        "weekday": "Friday",
        "close": "17156.6351"
    },
    {
        "date": "2017-07-08",
        "month": "07",
        "week": "27",
        "weekday": "Saturday",
        "close": "17557.352"
    },
    {
        "date": "2017-07-09",
        "month": "07",
        "week": "27",
        "weekday": "Sunday",
        "close": "17189.5013"
    },
    {
        "date": "2017-07-10",
        "month": "07",
        "week": "28",
        "weekday": "Monday",
        "close": "16137.2933"
    },
    {
        "date": "2017-07-11",
        "month": "07",
        "week": "28",
        "weekday": "Tuesday",
        "close": "15865.7291"
    },
    {
        "date": "2017-07-12",
        "month": "07",
        "week": "28",
        "weekday": "Wednesday",
        "close": "16446.9487"
    },
    {
        "date": "2017-07-13",
        "month": "07",
        "week": "28",
        "weekday": "Thursday",
        "close": "16036.6222"
    },
    {
        "date": "2017-07-14",
        "month": "07",
        "week": "28",
        "weekday": "Friday",
        "close": "15132.8235"
    },
    {
        "date": "2017-07-15",
        "month": "07",
        "week": "28",
        "weekday": "Saturday",
        "close": "13510.2081"
    },
    {
        "date": "2017-07-16",
        "month": "07",
        "week": "28",
        "weekday": "Sunday",
        "close": "13075.9378"
    },
    {
        "date": "2017-07-17",
        "month": "07",
        "week": "29",
        "weekday": "Monday",
        "close": "15192.6798"
    },
    {
        "date": "2017-07-18",
        "month": "07",
        "week": "29",
        "weekday": "Tuesday",
        "close": "15706.0407"
    },
    {
        "date": "2017-07-19",
        "month": "07",
        "week": "29",
        "weekday": "Wednesday",
        "close": "15491.8987"
    },
    {
        "date": "2017-07-20",
        "month": "07",
        "week": "29",
        "weekday": "Thursday",
        "close": "19449.5488"
    },
    {
        "date": "2017-07-21",
        "month": "07",
        "week": "29",
        "weekday": "Friday",
        "close": "18231.0571"
    },
    {
        "date": "2017-07-22",
        "month": "07",
        "week": "29",
        "weekday": "Saturday",
        "close": "19210.2278"
    },
    {
        "date": "2017-07-23",
        "month": "07",
        "week": "29",
        "weekday": "Sunday",
        "close": "18585.2759"
    },
    {
        "date": "2017-07-24",
        "month": "07",
        "week": "30",
        "weekday": "Monday",
        "close": "18762.6589"
    },
    {
        "date": "2017-07-25",
        "month": "07",
        "week": "30",
        "weekday": "Tuesday",
        "close": "17489.6893"
    },
    {
        "date": "2017-07-26",
        "month": "07",
        "week": "30",
        "weekday": "Wednesday",
        "close": "17219.7355"
    },
    {
        "date": "2017-07-27",
        "month": "07",
        "week": "30",
        "weekday": "Thursday",
        "close": "18188.4669"
    },
    {
        "date": "2017-07-28",
        "month": "07",
        "week": "30",
        "weekday": "Friday",
        "close": "18898.2088"
    },
    {
        "date": "2017-07-29",
        "month": "07",
        "week": "30",
        "weekday": "Saturday",
        "close": "18326.2673"
    },
    {
        "date": "2017-07-30",
        "month": "07",
        "week": "30",
        "weekday": "Sunday",
        "close": "18499.4572"
    },
    {
        "date": "2017-07-31",
        "month": "07",
        "week": "31",
        "weekday": "Monday",
        "close": "19334.0151"
    },
    {
        "date": "2017-08-01",
        "month": "08",
        "week": "31",
        "weekday": "Tuesday",
        "close": "18376.5093"
    },
    {
        "date": "2017-08-02",
        "month": "08",
        "week": "31",
        "weekday": "Wednesday",
        "close": "18305.9985"
    },
    {
        "date": "2017-08-03",
        "month": "08",
        "week": "31",
        "weekday": "Thursday",
        "close": "18901.8131"
    },
    {
        "date": "2017-08-04",
        "month": "08",
        "week": "31",
        "weekday": "Friday",
        "close": "19412.7978"
    },
    {
        "date": "2017-08-05",
        "month": "08",
        "week": "31",
        "weekday": "Saturday",
        "close": "22227.2745"
    },
    {
        "date": "2017-08-06",
        "month": "08",
        "week": "31",
        "weekday": "Sunday",
        "close": "22027.9885"
    },
    {
        "date": "2017-08-07",
        "month": "08",
        "week": "32",
        "weekday": "Monday",
        "close": "23063.1732"
    },
    {
        "date": "2017-08-08",
        "month": "08",
        "week": "32",
        "weekday": "Tuesday",
        "close": "23332.6502"
    },
    {
        "date": "2017-08-09",
        "month": "08",
        "week": "32",
        "weekday": "Wednesday",
        "close": "22541.6557"
    },
    {
        "date": "2017-08-10",
        "month": "08",
        "week": "32",
        "weekday": "Thursday",
        "close": "22904.393"
    },
    {
        "date": "2017-08-11",
        "month": "08",
        "week": "32",
        "weekday": "Friday",
        "close": "24526.2402"
    },
    {
        "date": "2017-08-12",
        "month": "08",
        "week": "32",
        "weekday": "Saturday",
        "close": "26109.954"
    },
    {
        "date": "2017-08-13",
        "month": "08",
        "week": "32",
        "weekday": "Sunday",
        "close": "27390.8412"
    },
    {
        "date": "2017-08-14",
        "month": "08",
        "week": "33",
        "weekday": "Monday",
        "close": "29237.7752"
    },
    {
        "date": "2017-08-15",
        "month": "08",
        "week": "33",
        "weekday": "Tuesday",
        "close": "28073.6691"
    },
    {
        "date": "2017-08-16",
        "month": "08",
        "week": "33",
        "weekday": "Wednesday",
        "close": "29612.7553"
    },
    {
        "date": "2017-08-17",
        "month": "08",
        "week": "33",
        "weekday": "Thursday",
        "close": "28816.4854"
    },
    {
        "date": "2017-08-18",
        "month": "08",
        "week": "33",
        "weekday": "Friday",
        "close": "27752.9395"
    },
    {
        "date": "2017-08-19",
        "month": "08",
        "week": "33",
        "weekday": "Saturday",
        "close": "28062.8866"
    },
    {
        "date": "2017-08-20",
        "month": "08",
        "week": "33",
        "weekday": "Sunday",
        "close": "27416.633"
    },
    {
        "date": "2017-08-21",
        "month": "08",
        "week": "34",
        "weekday": "Monday",
        "close": "26919.0691"
    },
    {
        "date": "2017-08-22",
        "month": "08",
        "week": "34",
        "weekday": "Tuesday",
        "close": "27565.3902"
    },
    {
        "date": "2017-08-23",
        "month": "08",
        "week": "34",
        "weekday": "Wednesday",
        "close": "27907.3345"
    },
    {
        "date": "2017-08-24",
        "month": "08",
        "week": "34",
        "weekday": "Thursday",
        "close": "29063.2438"
    },
    {
        "date": "2017-08-25",
        "month": "08",
        "week": "34",
        "weekday": "Friday",
        "close": "29305.655"
    },
    {
        "date": "2017-08-26",
        "month": "08",
        "week": "34",
        "weekday": "Saturday",
        "close": "29168.7202"
    },
    {
        "date": "2017-08-27",
        "month": "08",
        "week": "34",
        "weekday": "Sunday",
        "close": "28945.352"
    },
    {
        "date": "2017-08-28",
        "month": "08",
        "week": "35",
        "weekday": "Monday",
        "close": "29340.152"
    },
    {
        "date": "2017-08-29",
        "month": "08",
        "week": "35",
        "weekday": "Tuesday",
        "close": "30656.5006"
    },
    {
        "date": "2017-08-30",
        "month": "08",
        "week": "35",
        "weekday": "Wednesday",
        "close": "30532.943"
    },
    {
        "date": "2017-08-31",
        "month": "08",
        "week": "35",
        "weekday": "Thursday",
        "close": "31391.9005"
    },
    {
        "date": "2017-09-01",
        "month": "09",
        "week": "35",
        "weekday": "Friday",
        "close": "32482.9375"
    },
    {
        "date": "2017-09-02",
        "month": "09",
        "week": "35",
        "weekday": "Saturday",
        "close": "30470.2819"
    },
    {
        "date": "2017-09-03",
        "month": "09",
        "week": "35",
        "weekday": "Sunday",
        "close": "30336.7"
    },
    {
        "date": "2017-09-04",
        "month": "09",
        "week": "36",
        "weekday": "Monday",
        "close": "28208.2898"
    },
    {
        "date": "2017-09-05",
        "month": "09",
        "week": "36",
        "weekday": "Tuesday",
        "close": "28918.0205"
    },
    {
        "date": "2017-09-06",
        "month": "09",
        "week": "36",
        "weekday": "Wednesday",
        "close": "30181.6445"
    },
    {
        "date": "2017-09-07",
        "month": "09",
        "week": "36",
        "weekday": "Thursday",
        "close": "30089.0416"
    },
    {
        "date": "2017-09-08",
        "month": "09",
        "week": "36",
        "weekday": "Friday",
        "close": "27976.5645"
    },
    {
        "date": "2017-09-09",
        "month": "09",
        "week": "36",
        "weekday": "Saturday",
        "close": "27818.6782"
    },
    {
        "date": "2017-09-10",
        "month": "09",
        "week": "36",
        "weekday": "Sunday",
        "close": "27359.9317"
    },
    {
        "date": "2017-09-11",
        "month": "09",
        "week": "37",
        "weekday": "Monday",
        "close": "27351.0555"
    },
    {
        "date": "2017-09-12",
        "month": "09",
        "week": "37",
        "weekday": "Tuesday",
        "close": "27111.147"
    },
    {
        "date": "2017-09-13",
        "month": "09",
        "week": "37",
        "weekday": "Wednesday",
        "close": "25354.506"
    },
    {
        "date": "2017-09-14",
        "month": "09",
        "week": "37",
        "weekday": "Thursday",
        "close": "21152.8443"
    },
    {
        "date": "2017-09-15",
        "month": "09",
        "week": "37",
        "weekday": "Friday",
        "close": "24164.8636"
    },
    {
        "date": "2017-09-16",
        "month": "09",
        "week": "37",
        "weekday": "Saturday",
        "close": "24111.3645"
    },
    {
        "date": "2017-09-17",
        "month": "09",
        "week": "37",
        "weekday": "Sunday",
        "close": "24057.8213"
    },
    {
        "date": "2017-09-18",
        "month": "09",
        "week": "38",
        "weekday": "Monday",
        "close": "26737.3742"
    },
    {
        "date": "2017-09-19",
        "month": "09",
        "week": "38",
        "weekday": "Tuesday",
        "close": "25652.4813"
    },
    {
        "date": "2017-09-20",
        "month": "09",
        "week": "38",
        "weekday": "Wednesday",
        "close": "25361.3238"
    },
    {
        "date": "2017-09-21",
        "month": "09",
        "week": "38",
        "weekday": "Thursday",
        "close": "23804.5608"
    },
    {
        "date": "2017-09-22",
        "month": "09",
        "week": "38",
        "weekday": "Friday",
        "close": "23761.1198"
    },
    {
        "date": "2017-09-23",
        "month": "09",
        "week": "38",
        "weekday": "Saturday",
        "close": "24908.4204"
    },
    {
        "date": "2017-09-24",
        "month": "09",
        "week": "38",
        "weekday": "Sunday",
        "close": "24216.5269"
    },
    {
        "date": "2017-09-25",
        "month": "09",
        "week": "39",
        "weekday": "Monday",
        "close": "26007.1112"
    },
    {
        "date": "2017-09-26",
        "month": "09",
        "week": "39",
        "weekday": "Tuesday",
        "close": "25869.3194"
    },
    {
        "date": "2017-09-27",
        "month": "09",
        "week": "39",
        "weekday": "Wednesday",
        "close": "27955.6252"
    },
    {
        "date": "2017-09-28",
        "month": "09",
        "week": "39",
        "weekday": "Thursday",
        "close": "27882.4195"
    },
    {
        "date": "2017-09-29",
        "month": "09",
        "week": "39",
        "weekday": "Friday",
        "close": "27711.6948"
    },
    {
        "date": "2017-09-30",
        "month": "09",
        "week": "39",
        "weekday": "Saturday",
        "close": "28969.0962"
    },
    {
        "date": "2017-10-01",
        "month": "10",
        "week": "39",
        "weekday": "Sunday",
        "close": "29264.4926"
    },
    {
        "date": "2017-10-02",
        "month": "10",
        "week": "40",
        "weekday": "Monday",
        "close": "29295.7562"
    },
    {
        "date": "2017-10-03",
        "month": "10",
        "week": "40",
        "weekday": "Tuesday",
        "close": "28743.0928"
    },
    {
        "date": "2017-10-04",
        "month": "10",
        "week": "40",
        "weekday": "Wednesday",
        "close": "28120.5656"
    },
    {
        "date": "2017-10-05",
        "month": "10",
        "week": "40",
        "weekday": "Thursday",
        "close": "28764.0436"
    },
    {
        "date": "2017-10-06",
        "month": "10",
        "week": "40",
        "weekday": "Friday",
        "close": "29084.1981"
    },
    {
        "date": "2017-10-07",
        "month": "10",
        "week": "40",
        "weekday": "Saturday",
        "close": "29521.3602"
    },
    {
        "date": "2017-10-08",
        "month": "10",
        "week": "40",
        "weekday": "Sunday",
        "close": "30583.2886"
    },
    {
        "date": "2017-10-09",
        "month": "10",
        "week": "41",
        "weekday": "Monday",
        "close": "31622.869"
    },
    {
        "date": "2017-10-10",
        "month": "10",
        "week": "41",
        "weekday": "Tuesday",
        "close": "31243.3645"
    },
    {
        "date": "2017-10-11",
        "month": "10",
        "week": "41",
        "weekday": "Wednesday",
        "close": "31830.4848"
    },
    {
        "date": "2017-10-12",
        "month": "10",
        "week": "41",
        "weekday": "Thursday",
        "close": "35833.2539"
    },
    {
        "date": "2017-10-13",
        "month": "10",
        "week": "41",
        "weekday": "Friday",
        "close": "37106.6814"
    },
    {
        "date": "2017-10-14",
        "month": "10",
        "week": "41",
        "weekday": "Saturday",
        "close": "38222.2666"
    },
    {
        "date": "2017-10-15",
        "month": "10",
        "week": "41",
        "weekday": "Sunday",
        "close": "37517.0856"
    },
    {
        "date": "2017-10-16",
        "month": "10",
        "week": "42",
        "weekday": "Monday",
        "close": "37917.9925"
    },
    {
        "date": "2017-10-17",
        "month": "10",
        "week": "42",
        "weekday": "Tuesday",
        "close": "37060.3182"
    },
    {
        "date": "2017-10-18",
        "month": "10",
        "week": "42",
        "weekday": "Wednesday",
        "close": "36928.632"
    },
    {
        "date": "2017-10-19",
        "month": "10",
        "week": "42",
        "weekday": "Thursday",
        "close": "37704.4579"
    },
    {
        "date": "2017-10-20",
        "month": "10",
        "week": "42",
        "weekday": "Friday",
        "close": "39634.3994"
    },
    {
        "date": "2017-10-21",
        "month": "10",
        "week": "42",
        "weekday": "Saturday",
        "close": "39827.4189"
    },
    {
        "date": "2017-10-22",
        "month": "10",
        "week": "42",
        "weekday": "Sunday",
        "close": "39673.3776"
    },
    {
        "date": "2017-10-23",
        "month": "10",
        "week": "43",
        "weekday": "Monday",
        "close": "39144.7834"
    },
    {
        "date": "2017-10-24",
        "month": "10",
        "week": "43",
        "weekday": "Tuesday",
        "close": "36611.829"
    },
    {
        "date": "2017-10-25",
        "month": "10",
        "week": "43",
        "weekday": "Wednesday",
        "close": "38067.9613"
    },
    {
        "date": "2017-10-26",
        "month": "10",
        "week": "43",
        "weekday": "Thursday",
        "close": "39108.6475"
    },
    {
        "date": "2017-10-27",
        "month": "10",
        "week": "43",
        "weekday": "Friday",
        "close": "38353.9173"
    },
    {
        "date": "2017-10-28",
        "month": "10",
        "week": "43",
        "weekday": "Saturday",
        "close": "38122.1385"
    },
    {
        "date": "2017-10-29",
        "month": "10",
        "week": "43",
        "weekday": "Sunday",
        "close": "40925.4142"
    },
    {
        "date": "2017-10-30",
        "month": "10",
        "week": "44",
        "weekday": "Monday",
        "close": "40682.7268"
    },
    {
        "date": "2017-10-31",
        "month": "10",
        "week": "44",
        "weekday": "Tuesday",
        "close": "42779.3067"
    },
    {
        "date": "2017-11-01",
        "month": "11",
        "week": "44",
        "weekday": "Wednesday",
        "close": "44572.0627"
    },
    {
        "date": "2017-11-02",
        "month": "11",
        "week": "44",
        "weekday": "Thursday",
        "close": "46462.0654"
    },
    {
        "date": "2017-11-03",
        "month": "11",
        "week": "44",
        "weekday": "Friday",
        "close": "47518.0205"
    },
    {
        "date": "2017-11-04",
        "month": "11",
        "week": "44",
        "weekday": "Saturday",
        "close": "49047.8253"
    },
    {
        "date": "2017-11-05",
        "month": "11",
        "week": "44",
        "weekday": "Sunday",
        "close": "48907.9843"
    },
    {
        "date": "2017-11-06",
        "month": "11",
        "week": "45",
        "weekday": "Monday",
        "close": "46159.7307"
    },
    {
        "date": "2017-11-07",
        "month": "11",
        "week": "45",
        "weekday": "Tuesday",
        "close": "47249.3415"
    },
    {
        "date": "2017-11-08",
        "month": "11",
        "week": "45",
        "weekday": "Wednesday",
        "close": "49427.7048"
    },
    {
        "date": "2017-11-09",
        "month": "11",
        "week": "45",
        "weekday": "Thursday",
        "close": "47448.9118"
    },
    {
        "date": "2017-11-10",
        "month": "11",
        "week": "45",
        "weekday": "Friday",
        "close": "43637.0596"
    },
    {
        "date": "2017-11-11",
        "month": "11",
        "week": "45",
        "weekday": "Saturday",
        "close": "42085.6019"
    },
    {
        "date": "2017-11-12",
        "month": "11",
        "week": "45",
        "weekday": "Sunday",
        "close": "38904.304"
    },
    {
        "date": "2017-11-13",
        "month": "11",
        "week": "46",
        "weekday": "Monday",
        "close": "43279.9771"
    },
    {
        "date": "2017-11-14",
        "month": "11",
        "week": "46",
        "weekday": "Tuesday",
        "close": "43801.9668"
    },
    {
        "date": "2017-11-15",
        "month": "11",
        "week": "46",
        "weekday": "Wednesday",
        "close": "48232.4671"
    },
    {
        "date": "2017-11-16",
        "month": "11",
        "week": "46",
        "weekday": "Thursday",
        "close": "52012.2859"
    },
    {
        "date": "2017-11-17",
        "month": "11",
        "week": "46",
        "weekday": "Friday",
        "close": "50973.7739"
    },
    {
        "date": "2017-11-18",
        "month": "11",
        "week": "46",
        "weekday": "Saturday",
        "close": "51542.0595"
    },
    {
        "date": "2017-11-19",
        "month": "11",
        "week": "46",
        "weekday": "Sunday",
        "close": "53279.4604"
    },
    {
        "date": "2017-11-20",
        "month": "11",
        "week": "47",
        "weekday": "Monday",
        "close": "54656.3545"
    },
    {
        "date": "2017-11-21",
        "month": "11",
        "week": "47",
        "weekday": "Tuesday",
        "close": "53673.7877"
    },
    {
        "date": "2017-11-22",
        "month": "11",
        "week": "47",
        "weekday": "Wednesday",
        "close": "54403.2305"
    },
    {
        "date": "2017-11-23",
        "month": "11",
        "week": "47",
        "weekday": "Thursday",
        "close": "52676.5845"
    },
    {
        "date": "2017-11-24",
        "month": "11",
        "week": "47",
        "weekday": "Friday",
        "close": "54136.6166"
    },
    {
        "date": "2017-11-25",
        "month": "11",
        "week": "47",
        "weekday": "Saturday",
        "close": "57851.0594"
    },
    {
        "date": "2017-11-26",
        "month": "11",
        "week": "47",
        "weekday": "Sunday",
        "close": "60980.0178"
    },
    {
        "date": "2017-11-27",
        "month": "11",
        "week": "48",
        "weekday": "Monday",
        "close": "64246.5971"
    },
    {
        "date": "2017-11-28",
        "month": "11",
        "week": "48",
        "weekday": "Tuesday",
        "close": "65458.8612"
    },
    {
        "date": "2017-11-29",
        "month": "11",
        "week": "48",
        "weekday": "Wednesday",
        "close": "64890.9642"
    },
    {
        "date": "2017-11-30",
        "month": "11",
        "week": "48",
        "weekday": "Thursday",
        "close": "65583.2597"
    },
    {
        "date": "2017-12-01",
        "month": "12",
        "week": "48",
        "weekday": "Friday",
        "close": "71825.6883"
    },
    {
        "date": "2017-12-02",
        "month": "12",
        "week": "48",
        "weekday": "Saturday",
        "close": "72079.2312"
    },
    {
        "date": "2017-12-03",
        "month": "12",
        "week": "48",
        "weekday": "Sunday",
        "close": "74007.4136"
    },
    {
        "date": "2017-12-04",
        "month": "12",
        "week": "49",
        "weekday": "Monday",
        "close": "76852.0129"
    },
    {
        "date": "2017-12-05",
        "month": "12",
        "week": "49",
        "weekday": "Tuesday",
        "close": "77398.8752"
    },
    {
        "date": "2017-12-06",
        "month": "12",
        "week": "49",
        "weekday": "Wednesday",
        "close": "90679.5487"
    },
    {
        "date": "2017-12-07",
        "month": "12",
        "week": "49",
        "weekday": "Thursday",
        "close": "111589.9776"
    },
    {
        "date": "2017-12-08",
        "month": "12",
        "week": "49",
        "weekday": "Friday",
        "close": "106233.201"
    },
    {
        "date": "2017-12-09",
        "month": "12",
        "week": "49",
        "weekday": "Saturday",
        "close": "98676.7747"
    },
    {
        "date": "2017-12-10",
        "month": "12",
        "week": "49",
        "weekday": "Sunday",
        "close": "99525.1027"
    },
    {
        "date": "2017-12-11",
        "month": "12",
        "week": "50",
        "weekday": "Monday",
        "close": "110642.88"
    },
    {
        "date": "2017-12-12",
        "month": "12",
        "week": "50",
        "weekday": "Tuesday",
        "close": "113732.6745"
    }]
    


```python
print(file_requests)
```

    [{'date': '2017-01-01', 'month': '01', 'week': '52', 'weekday': 'Sunday', 'close': '6928.6492'}, {'date': '2017-01-02', 'month': '01', 'week': '1', 'weekday': 'Monday', 'close': '7070.2554'}, {'date': '2017-01-03', 'month': '01', 'week': '1', 'weekday': 'Tuesday', 'close': '7175.1082'}, {'date': '2017-01-04', 'month': '01', 'week': '1', 'weekday': 'Wednesday', 'close': '7835.7615'}, {'date': '2017-01-05', 'month': '01', 'week': '1', 'weekday': 'Thursday', 'close': '6928.7578'}, {'date': '2017-01-06', 'month': '01', 'week': '1', 'weekday': 'Friday', 'close': '6196.6928'}, {'date': '2017-01-07', 'month': '01', 'week': '1', 'weekday': 'Saturday', 'close': '6262.1471'}, {'date': '2017-01-08', 'month': '01', 'week': '1', 'weekday': 'Sunday', 'close': '6319.9404'}, {'date': '2017-01-09', 'month': '01', 'week': '2', 'weekday': 'Monday', 'close': '6239.1506'}, {'date': '2017-01-10', 'month': '01', 'week': '2', 'weekday': 'Tuesday', 'close': '6263.1548'}, {'date': '2017-01-11', 'month': '01', 'week': '2', 'weekday': 'Wednesday', 'close': '5383.0598'}, {'date': '2017-01-12', 'month': '01', 'week': '2', 'weekday': 'Thursday', 'close': '5566.7345'}, {'date': '2017-01-13', 'month': '01', 'week': '2', 'weekday': 'Friday', 'close': '5700.0716'}, {'date': '2017-01-14', 'month': '01', 'week': '2', 'weekday': 'Saturday', 'close': '5648.6897'}, {'date': '2017-01-15', 'month': '01', 'week': '2', 'weekday': 'Sunday', 'close': '5674.7977'}, {'date': '2017-01-16', 'month': '01', 'week': '3', 'weekday': 'Monday', 'close': '5730.0658'}, {'date': '2017-01-17', 'month': '01', 'week': '3', 'weekday': 'Tuesday', 'close': '6202.9704'}, {'date': '2017-01-18', 'month': '01', 'week': '3', 'weekday': 'Wednesday', 'close': '6047.6601'}, {'date': '2017-01-19', 'month': '01', 'week': '3', 'weekday': 'Thursday', 'close': '6170.8433'}, {'date': '2017-01-20', 'month': '01', 'week': '3', 'weekday': 'Friday', 'close': '6131.2511'}, {'date': '2017-01-21', 'month': '01', 'week': '3', 'weekday': 'Saturday', 'close': '6326.3657'}, {'date': '2017-01-22', 'month': '01', 'week': '3', 'weekday': 'Sunday', 'close': '6362.9482'}, {'date': '2017-01-23', 'month': '01', 'week': '4', 'weekday': 'Monday', 'close': '6255.5602'}, {'date': '2017-01-24', 'month': '01', 'week': '4', 'weekday': 'Tuesday', 'close': '6074.8333'}, {'date': '2017-01-25', 'month': '01', 'week': '4', 'weekday': 'Wednesday', 'close': '6154.6958'}, {'date': '2017-01-26', 'month': '01', 'week': '4', 'weekday': 'Thursday', 'close': '6295.3388'}, {'date': '2017-01-27', 'month': '01', 'week': '4', 'weekday': 'Friday', 'close': '6320.7206'}, {'date': '2017-01-28', 'month': '01', 'week': '4', 'weekday': 'Saturday', 'close': '6332.5389'}, {'date': '2017-01-29', 'month': '01', 'week': '4', 'weekday': 'Sunday', 'close': '6289.1698'}, {'date': '2017-01-30', 'month': '01', 'week': '5', 'weekday': 'Monday', 'close': '6332.8246'}, {'date': '2017-01-31', 'month': '01', 'week': '5', 'weekday': 'Tuesday', 'close': '6657.8667'}, {'date': '2017-02-01', 'month': '02', 'week': '5', 'weekday': 'Wednesday', 'close': '6793.7077'}, {'date': '2017-02-02', 'month': '02', 'week': '5', 'weekday': 'Thursday', 'close': '6934.3856'}, {'date': '2017-02-03', 'month': '02', 'week': '5', 'weekday': 'Friday', 'close': '6995.2901'}, {'date': '2017-02-04', 'month': '02', 'week': '5', 'weekday': 'Saturday', 'close': '7102.0714'}, {'date': '2017-02-05', 'month': '02', 'week': '5', 'weekday': 'Sunday', 'close': '6965.9773'}, {'date': '2017-02-06', 'month': '02', 'week': '6', 'weekday': 'Monday', 'close': '7034.2211'}, {'date': '2017-02-07', 'month': '02', 'week': '6', 'weekday': 'Tuesday', 'close': '7245.8877'}, {'date': '2017-02-08', 'month': '02', 'week': '6', 'weekday': 'Wednesday', 'close': '7246.6303'}, {'date': '2017-02-09', 'month': '02', 'week': '6', 'weekday': 'Thursday', 'close': '6811.6794'}, {'date': '2017-02-10', 'month': '02', 'week': '6', 'weekday': 'Friday', 'close': '6833.4884'}, {'date': '2017-02-11', 'month': '02', 'week': '6', 'weekday': 'Saturday', 'close': '6946.09'}, {'date': '2017-02-12', 'month': '02', 'week': '6', 'weekday': 'Sunday', 'close': '6883.9424'}, {'date': '2017-02-13', 'month': '02', 'week': '7', 'weekday': 'Monday', 'close': '6858.5789'}, {'date': '2017-02-14', 'month': '02', 'week': '7', 'weekday': 'Tuesday', 'close': '6930.882'}, {'date': '2017-02-15', 'month': '02', 'week': '7', 'weekday': 'Wednesday', 'close': '6935.3788'}, {'date': '2017-02-16', 'month': '02', 'week': '7', 'weekday': 'Thursday', 'close': '7088.8535'}, {'date': '2017-02-17', 'month': '02', 'week': '7', 'weekday': 'Friday', 'close': '7229.5808'}, {'date': '2017-02-18', 'month': '02', 'week': '7', 'weekday': 'Saturday', 'close': '7267.5468'}, {'date': '2017-02-19', 'month': '02', 'week': '7', 'weekday': 'Sunday', 'close': '7220.5385'}, {'date': '2017-02-20', 'month': '02', 'week': '8', 'weekday': 'Monday', 'close': '7450.2901'}, {'date': '2017-02-21', 'month': '02', 'week': '8', 'weekday': 'Tuesday', 'close': '7732.4979'}, {'date': '2017-02-22', 'month': '02', 'week': '8', 'weekday': 'Wednesday', 'close': '7716.2218'}, {'date': '2017-02-23', 'month': '02', 'week': '8', 'weekday': 'Thursday', 'close': '8092.0221'}, {'date': '2017-02-24', 'month': '02', 'week': '8', 'weekday': 'Friday', 'close': '8109.1867'}, {'date': '2017-02-25', 'month': '02', 'week': '8', 'weekday': 'Saturday', 'close': '7908.54'}, {'date': '2017-02-26', 'month': '02', 'week': '8', 'weekday': 'Sunday', 'close': '8137.4131'}, {'date': '2017-02-27', 'month': '02', 'week': '9', 'weekday': 'Monday', 'close': '8206.1829'}, {'date': '2017-02-28', 'month': '02', 'week': '9', 'weekday': 'Tuesday', 'close': '8176.3692'}, {'date': '2017-03-01', 'month': '03', 'week': '9', 'weekday': 'Wednesday', 'close': '8464.3549'}, {'date': '2017-03-02', 'month': '03', 'week': '9', 'weekday': 'Thursday', 'close': '8688.7751'}, {'date': '2017-03-03', 'month': '03', 'week': '9', 'weekday': 'Friday', 'close': '8900.4858'}, {'date': '2017-03-04', 'month': '03', 'week': '9', 'weekday': 'Saturday', 'close': '8741.0338'}, {'date': '2017-03-05', 'month': '03', 'week': '9', 'weekday': 'Sunday', 'close': '8816.6651'}, {'date': '2017-03-06', 'month': '03', 'week': '10', 'weekday': 'Monday', 'close': '8832.9615'}, {'date': '2017-03-07', 'month': '03', 'week': '10', 'weekday': 'Tuesday', 'close': '8504.1113'}, {'date': '2017-03-08', 'month': '03', 'week': '10', 'weekday': 'Wednesday', 'close': '7953.9243'}, {'date': '2017-03-09', 'month': '03', 'week': '10', 'weekday': 'Thursday', 'close': '8235.799'}, {'date': '2017-03-10', 'month': '03', 'week': '10', 'weekday': 'Friday', 'close': '7716.1296'}, {'date': '2017-03-11', 'month': '03', 'week': '10', 'weekday': 'Saturday', 'close': '8161.7404'}, {'date': '2017-03-12', 'month': '03', 'week': '10', 'weekday': 'Sunday', 'close': '8441.5353'}, {'date': '2017-03-13', 'month': '03', 'week': '11', 'weekday': 'Monday', 'close': '8595.5263'}, {'date': '2017-03-14', 'month': '03', 'week': '11', 'weekday': 'Tuesday', 'close': '8616.949'}, {'date': '2017-03-15', 'month': '03', 'week': '11', 'weekday': 'Wednesday', 'close': '8711.306'}, {'date': '2017-03-16', 'month': '03', 'week': '11', 'weekday': 'Thursday', 'close': '8091.7411'}, {'date': '2017-03-17', 'month': '03', 'week': '11', 'weekday': 'Friday', 'close': '7379.6562'}, {'date': '2017-03-18', 'month': '03', 'week': '11', 'weekday': 'Saturday', 'close': '6694.36'}, {'date': '2017-03-19', 'month': '03', 'week': '11', 'weekday': 'Sunday', 'close': '7028.0107'}, {'date': '2017-03-20', 'month': '03', 'week': '12', 'weekday': 'Monday', 'close': '7196.3568'}, {'date': '2017-03-21', 'month': '03', 'week': '12', 'weekday': 'Tuesday', 'close': '7680.723'}, {'date': '2017-03-22', 'month': '03', 'week': '12', 'weekday': 'Wednesday', 'close': '7139.7016'}, {'date': '2017-03-23', 'month': '03', 'week': '12', 'weekday': 'Thursday', 'close': '7092.2246'}, {'date': '2017-03-24', 'month': '03', 'week': '12', 'weekday': 'Friday', 'close': '6437.3431'}, {'date': '2017-03-25', 'month': '03', 'week': '12', 'weekday': 'Saturday', 'close': '6640.554'}, {'date': '2017-03-26', 'month': '03', 'week': '12', 'weekday': 'Sunday', 'close': '6623.5896'}, {'date': '2017-03-27', 'month': '03', 'week': '13', 'weekday': 'Monday', 'close': '7151.8202'}, {'date': '2017-03-28', 'month': '03', 'week': '13', 'weekday': 'Tuesday', 'close': '7184.6725'}, {'date': '2017-03-29', 'month': '03', 'week': '13', 'weekday': 'Wednesday', 'close': '7168.8792'}, {'date': '2017-03-30', 'month': '03', 'week': '13', 'weekday': 'Thursday', 'close': '7146.3119'}, {'date': '2017-03-31', 'month': '03', 'week': '13', 'weekday': 'Friday', 'close': '7439.1397'}, {'date': '2017-04-01', 'month': '04', 'week': '13', 'weekday': 'Saturday', 'close': '7506.4038'}, {'date': '2017-04-02', 'month': '04', 'week': '13', 'weekday': 'Sunday', 'close': '7566.0156'}, {'date': '2017-04-03', 'month': '04', 'week': '14', 'weekday': 'Monday', 'close': '7903.6773'}, {'date': '2017-04-04', 'month': '04', 'week': '14', 'weekday': 'Tuesday', 'close': '7874.9773'}, {'date': '2017-04-05', 'month': '04', 'week': '14', 'weekday': 'Wednesday', 'close': '7827.8202'}, {'date': '2017-04-06', 'month': '04', 'week': '14', 'weekday': 'Thursday', 'close': '8212.9762'}, {'date': '2017-04-07', 'month': '04', 'week': '14', 'weekday': 'Friday', 'close': '8236.9016'}, {'date': '2017-04-08', 'month': '04', 'week': '14', 'weekday': 'Saturday', 'close': '8180.3212'}, {'date': '2017-04-09', 'month': '04', 'week': '14', 'weekday': 'Sunday', 'close': '8354.8293'}, {'date': '2017-04-10', 'month': '04', 'week': '15', 'weekday': 'Monday', 'close': '8375.1346'}, {'date': '2017-04-11', 'month': '04', 'week': '15', 'weekday': 'Tuesday', 'close': '8442.2186'}, {'date': '2017-04-12', 'month': '04', 'week': '15', 'weekday': 'Wednesday', 'close': '8382.4873'}, {'date': '2017-04-13', 'month': '04', 'week': '15', 'weekday': 'Thursday', 'close': '8117.4785'}, {'date': '2017-04-14', 'month': '04', 'week': '15', 'weekday': 'Friday', 'close': '8151.1798'}, {'date': '2017-04-15', 'month': '04', 'week': '15', 'weekday': 'Saturday', 'close': '8129.5847'}, {'date': '2017-04-16', 'month': '04', 'week': '15', 'weekday': 'Sunday', 'close': '8167.0471'}, {'date': '2017-04-17', 'month': '04', 'week': '16', 'weekday': 'Monday', 'close': '8267.104'}, {'date': '2017-04-18', 'month': '04', 'week': '16', 'weekday': 'Tuesday', 'close': '8379.7232'}, {'date': '2017-04-19', 'month': '04', 'week': '16', 'weekday': 'Wednesday', 'close': '8436.3248'}, {'date': '2017-04-20', 'month': '04', 'week': '16', 'weekday': 'Thursday', 'close': '8639.4949'}, {'date': '2017-04-21', 'month': '04', 'week': '16', 'weekday': 'Friday', 'close': '8654.9971'}, {'date': '2017-04-22', 'month': '04', 'week': '16', 'weekday': 'Saturday', 'close': '8567.1483'}, {'date': '2017-04-23', 'month': '04', 'week': '16', 'weekday': 'Sunday', 'close': '8458.4188'}, {'date': '2017-04-24', 'month': '04', 'week': '17', 'weekday': 'Monday', 'close': '8594.2345'}, {'date': '2017-04-25', 'month': '04', 'week': '17', 'weekday': 'Tuesday', 'close': '8700.0125'}, {'date': '2017-04-26', 'month': '04', 'week': '17', 'weekday': 'Wednesday', 'close': '8857.1946'}, {'date': '2017-04-27', 'month': '04', 'week': '17', 'weekday': 'Thursday', 'close': '9167.2508'}, {'date': '2017-04-28', 'month': '04', 'week': '17', 'weekday': 'Friday', 'close': '9101.0934'}, {'date': '2017-04-29', 'month': '04', 'week': '17', 'weekday': 'Saturday', 'close': '9149.9325'}, {'date': '2017-04-30', 'month': '04', 'week': '17', 'weekday': 'Sunday', 'close': '9325.1119'}, {'date': '2017-05-01', 'month': '05', 'week': '18', 'weekday': 'Monday', 'close': '9665.7551'}, {'date': '2017-05-02', 'month': '05', 'week': '18', 'weekday': 'Tuesday', 'close': '9944.3653'}, {'date': '2017-05-03', 'month': '05', 'week': '18', 'weekday': 'Wednesday', 'close': '10292.3296'}, {'date': '2017-05-04', 'month': '05', 'week': '18', 'weekday': 'Thursday', 'close': '10452.0037'}, {'date': '2017-05-05', 'month': '05', 'week': '18', 'weekday': 'Friday', 'close': '10439.0799'}, {'date': '2017-05-06', 'month': '05', 'week': '18', 'weekday': 'Saturday', 'close': '10688.1301'}, {'date': '2017-05-07', 'month': '05', 'week': '18', 'weekday': 'Sunday', 'close': '10660.1939'}, {'date': '2017-05-08', 'month': '05', 'week': '19', 'weekday': 'Monday', 'close': '11317.8009'}, {'date': '2017-05-09', 'month': '05', 'week': '19', 'weekday': 'Tuesday', 'close': '11794.8949'}, {'date': '2017-05-10', 'month': '05', 'week': '19', 'weekday': 'Wednesday', 'close': '12126.2961'}, {'date': '2017-05-11', 'month': '05', 'week': '19', 'weekday': 'Thursday', 'close': '12478.0838'}, {'date': '2017-05-12', 'month': '05', 'week': '19', 'weekday': 'Friday', 'close': '11569.4125'}, {'date': '2017-05-13', 'month': '05', 'week': '19', 'weekday': 'Saturday', 'close': '12141.797'}, {'date': '2017-05-14', 'month': '05', 'week': '19', 'weekday': 'Sunday', 'close': '12229.3176'}, {'date': '2017-05-15', 'month': '05', 'week': '20', 'weekday': 'Monday', 'close': '11701.2204'}, {'date': '2017-05-16', 'month': '05', 'week': '20', 'weekday': 'Tuesday', 'close': '11835.218'}, {'date': '2017-05-17', 'month': '05', 'week': '20', 'weekday': 'Wednesday', 'close': '12403.3024'}, {'date': '2017-05-18', 'month': '05', 'week': '20', 'weekday': 'Thursday', 'close': '13002.0625'}, {'date': '2017-05-19', 'month': '05', 'week': '20', 'weekday': 'Friday', 'close': '13549.3033'}, {'date': '2017-05-20', 'month': '05', 'week': '20', 'weekday': 'Saturday', 'close': '14127.3239'}, {'date': '2017-05-21', 'month': '05', 'week': '20', 'weekday': 'Sunday', 'close': '14091.8068'}, {'date': '2017-05-22', 'month': '05', 'week': '21', 'weekday': 'Monday', 'close': '14731.8028'}, {'date': '2017-05-23', 'month': '05', 'week': '21', 'weekday': 'Tuesday', 'close': '15784.8432'}, {'date': '2017-05-24', 'month': '05', 'week': '21', 'weekday': 'Wednesday', 'close': '17061.8818'}, {'date': '2017-05-25', 'month': '05', 'week': '21', 'weekday': 'Thursday', 'close': '16190.3931'}, {'date': '2017-05-26', 'month': '05', 'week': '21', 'weekday': 'Friday', 'close': '15402.2219'}, {'date': '2017-05-27', 'month': '05', 'week': '21', 'weekday': 'Saturday', 'close': '14440.0015'}, {'date': '2017-05-28', 'month': '05', 'week': '21', 'weekday': 'Sunday', 'close': '15139.4071'}, {'date': '2017-05-29', 'month': '05', 'week': '22', 'weekday': 'Monday', 'close': '15700.3794'}, {'date': '2017-05-30', 'month': '05', 'week': '22', 'weekday': 'Tuesday', 'close': '15064.5355'}, {'date': '2017-05-31', 'month': '05', 'week': '22', 'weekday': 'Wednesday', 'close': '15869.5798'}, {'date': '2017-06-01', 'month': '06', 'week': '22', 'weekday': 'Thursday', 'close': '16693.6332'}, {'date': '2017-06-02', 'month': '06', 'week': '22', 'weekday': 'Friday', 'close': '17149.9736'}, {'date': '2017-06-03', 'month': '06', 'week': '22', 'weekday': 'Saturday', 'close': '17410.0077'}, {'date': '2017-06-04', 'month': '06', 'week': '22', 'weekday': 'Sunday', 'close': '17399.0513'}, {'date': '2017-06-05', 'month': '06', 'week': '23', 'weekday': 'Monday', 'close': '18621.161'}, {'date': '2017-06-06', 'month': '06', 'week': '23', 'weekday': 'Tuesday', 'close': '19797.8391'}, {'date': '2017-06-07', 'month': '06', 'week': '23', 'weekday': 'Wednesday', 'close': '18205.3747'}, {'date': '2017-06-08', 'month': '06', 'week': '23', 'weekday': 'Thursday', 'close': '19209.0831'}, {'date': '2017-06-09', 'month': '06', 'week': '23', 'weekday': 'Friday', 'close': '19218.5925'}, {'date': '2017-06-10', 'month': '06', 'week': '23', 'weekday': 'Saturday', 'close': '20004.1207'}, {'date': '2017-06-11', 'month': '06', 'week': '23', 'weekday': 'Sunday', 'close': '20472.3611'}, {'date': '2017-06-12', 'month': '06', 'week': '24', 'weekday': 'Monday', 'close': '18234.4754'}, {'date': '2017-06-13', 'month': '06', 'week': '24', 'weekday': 'Tuesday', 'close': '18615.1877'}, {'date': '2017-06-14', 'month': '06', 'week': '24', 'weekday': 'Wednesday', 'close': '16946.0339'}, {'date': '2017-06-15', 'month': '06', 'week': '24', 'weekday': 'Thursday', 'close': '16724.4891'}, {'date': '2017-06-16', 'month': '06', 'week': '24', 'weekday': 'Friday', 'close': '17217.0095'}, {'date': '2017-06-17', 'month': '06', 'week': '24', 'weekday': 'Saturday', 'close': '18142.6219'}, {'date': '2017-06-18', 'month': '06', 'week': '24', 'weekday': 'Sunday', 'close': '17535.8535'}, {'date': '2017-06-19', 'month': '06', 'week': '25', 'weekday': 'Monday', 'close': '18015.1039'}, {'date': '2017-06-20', 'month': '06', 'week': '25', 'weekday': 'Tuesday', 'close': '18975.7796'}, {'date': '2017-06-21', 'month': '06', 'week': '25', 'weekday': 'Wednesday', 'close': '18522.6802'}, {'date': '2017-06-22', 'month': '06', 'week': '25', 'weekday': 'Thursday', 'close': '18733.2802'}, {'date': '2017-06-23', 'month': '06', 'week': '25', 'weekday': 'Friday', 'close': '18720.4229'}, {'date': '2017-06-24', 'month': '06', 'week': '25', 'weekday': 'Saturday', 'close': '17906.1295'}, {'date': '2017-06-25', 'month': '06', 'week': '25', 'weekday': 'Sunday', 'close': '17734.3884'}, {'date': '2017-06-26', 'month': '06', 'week': '26', 'weekday': 'Monday', 'close': '17001.592'}, {'date': '2017-06-27', 'month': '06', 'week': '26', 'weekday': 'Tuesday', 'close': '17666.3417'}, {'date': '2017-06-28', 'month': '06', 'week': '26', 'weekday': 'Wednesday', 'close': '17575.261'}, {'date': '2017-06-29', 'month': '06', 'week': '26', 'weekday': 'Thursday', 'close': '17385.3171'}, {'date': '2017-06-30', 'month': '06', 'week': '26', 'weekday': 'Friday', 'close': '16943.0147'}, {'date': '2017-07-01', 'month': '07', 'week': '26', 'weekday': 'Saturday', 'close': '16674.129'}, {'date': '2017-07-02', 'month': '07', 'week': '26', 'weekday': 'Sunday', 'close': '17150.7103'}, {'date': '2017-07-03', 'month': '07', 'week': '27', 'weekday': 'Monday', 'close': '17549.3179'}, {'date': '2017-07-04', 'month': '07', 'week': '27', 'weekday': 'Tuesday', 'close': '17851.5456'}, {'date': '2017-07-05', 'month': '07', 'week': '27', 'weekday': 'Wednesday', 'close': '17812.6481'}, {'date': '2017-07-06', 'month': '07', 'week': '27', 'weekday': 'Thursday', 'close': '17813.1077'}, {'date': '2017-07-07', 'month': '07', 'week': '27', 'weekday': 'Friday', 'close': '17156.6351'}, {'date': '2017-07-08', 'month': '07', 'week': '27', 'weekday': 'Saturday', 'close': '17557.352'}, {'date': '2017-07-09', 'month': '07', 'week': '27', 'weekday': 'Sunday', 'close': '17189.5013'}, {'date': '2017-07-10', 'month': '07', 'week': '28', 'weekday': 'Monday', 'close': '16137.2933'}, {'date': '2017-07-11', 'month': '07', 'week': '28', 'weekday': 'Tuesday', 'close': '15865.7291'}, {'date': '2017-07-12', 'month': '07', 'week': '28', 'weekday': 'Wednesday', 'close': '16446.9487'}, {'date': '2017-07-13', 'month': '07', 'week': '28', 'weekday': 'Thursday', 'close': '16036.6222'}, {'date': '2017-07-14', 'month': '07', 'week': '28', 'weekday': 'Friday', 'close': '15132.8235'}, {'date': '2017-07-15', 'month': '07', 'week': '28', 'weekday': 'Saturday', 'close': '13510.2081'}, {'date': '2017-07-16', 'month': '07', 'week': '28', 'weekday': 'Sunday', 'close': '13075.9378'}, {'date': '2017-07-17', 'month': '07', 'week': '29', 'weekday': 'Monday', 'close': '15192.6798'}, {'date': '2017-07-18', 'month': '07', 'week': '29', 'weekday': 'Tuesday', 'close': '15706.0407'}, {'date': '2017-07-19', 'month': '07', 'week': '29', 'weekday': 'Wednesday', 'close': '15491.8987'}, {'date': '2017-07-20', 'month': '07', 'week': '29', 'weekday': 'Thursday', 'close': '19449.5488'}, {'date': '2017-07-21', 'month': '07', 'week': '29', 'weekday': 'Friday', 'close': '18231.0571'}, {'date': '2017-07-22', 'month': '07', 'week': '29', 'weekday': 'Saturday', 'close': '19210.2278'}, {'date': '2017-07-23', 'month': '07', 'week': '29', 'weekday': 'Sunday', 'close': '18585.2759'}, {'date': '2017-07-24', 'month': '07', 'week': '30', 'weekday': 'Monday', 'close': '18762.6589'}, {'date': '2017-07-25', 'month': '07', 'week': '30', 'weekday': 'Tuesday', 'close': '17489.6893'}, {'date': '2017-07-26', 'month': '07', 'week': '30', 'weekday': 'Wednesday', 'close': '17219.7355'}, {'date': '2017-07-27', 'month': '07', 'week': '30', 'weekday': 'Thursday', 'close': '18188.4669'}, {'date': '2017-07-28', 'month': '07', 'week': '30', 'weekday': 'Friday', 'close': '18898.2088'}, {'date': '2017-07-29', 'month': '07', 'week': '30', 'weekday': 'Saturday', 'close': '18326.2673'}, {'date': '2017-07-30', 'month': '07', 'week': '30', 'weekday': 'Sunday', 'close': '18499.4572'}, {'date': '2017-07-31', 'month': '07', 'week': '31', 'weekday': 'Monday', 'close': '19334.0151'}, {'date': '2017-08-01', 'month': '08', 'week': '31', 'weekday': 'Tuesday', 'close': '18376.5093'}, {'date': '2017-08-02', 'month': '08', 'week': '31', 'weekday': 'Wednesday', 'close': '18305.9985'}, {'date': '2017-08-03', 'month': '08', 'week': '31', 'weekday': 'Thursday', 'close': '18901.8131'}, {'date': '2017-08-04', 'month': '08', 'week': '31', 'weekday': 'Friday', 'close': '19412.7978'}, {'date': '2017-08-05', 'month': '08', 'week': '31', 'weekday': 'Saturday', 'close': '22227.2745'}, {'date': '2017-08-06', 'month': '08', 'week': '31', 'weekday': 'Sunday', 'close': '22027.9885'}, {'date': '2017-08-07', 'month': '08', 'week': '32', 'weekday': 'Monday', 'close': '23063.1732'}, {'date': '2017-08-08', 'month': '08', 'week': '32', 'weekday': 'Tuesday', 'close': '23332.6502'}, {'date': '2017-08-09', 'month': '08', 'week': '32', 'weekday': 'Wednesday', 'close': '22541.6557'}, {'date': '2017-08-10', 'month': '08', 'week': '32', 'weekday': 'Thursday', 'close': '22904.393'}, {'date': '2017-08-11', 'month': '08', 'week': '32', 'weekday': 'Friday', 'close': '24526.2402'}, {'date': '2017-08-12', 'month': '08', 'week': '32', 'weekday': 'Saturday', 'close': '26109.954'}, {'date': '2017-08-13', 'month': '08', 'week': '32', 'weekday': 'Sunday', 'close': '27390.8412'}, {'date': '2017-08-14', 'month': '08', 'week': '33', 'weekday': 'Monday', 'close': '29237.7752'}, {'date': '2017-08-15', 'month': '08', 'week': '33', 'weekday': 'Tuesday', 'close': '28073.6691'}, {'date': '2017-08-16', 'month': '08', 'week': '33', 'weekday': 'Wednesday', 'close': '29612.7553'}, {'date': '2017-08-17', 'month': '08', 'week': '33', 'weekday': 'Thursday', 'close': '28816.4854'}, {'date': '2017-08-18', 'month': '08', 'week': '33', 'weekday': 'Friday', 'close': '27752.9395'}, {'date': '2017-08-19', 'month': '08', 'week': '33', 'weekday': 'Saturday', 'close': '28062.8866'}, {'date': '2017-08-20', 'month': '08', 'week': '33', 'weekday': 'Sunday', 'close': '27416.633'}, {'date': '2017-08-21', 'month': '08', 'week': '34', 'weekday': 'Monday', 'close': '26919.0691'}, {'date': '2017-08-22', 'month': '08', 'week': '34', 'weekday': 'Tuesday', 'close': '27565.3902'}, {'date': '2017-08-23', 'month': '08', 'week': '34', 'weekday': 'Wednesday', 'close': '27907.3345'}, {'date': '2017-08-24', 'month': '08', 'week': '34', 'weekday': 'Thursday', 'close': '29063.2438'}, {'date': '2017-08-25', 'month': '08', 'week': '34', 'weekday': 'Friday', 'close': '29305.655'}, {'date': '2017-08-26', 'month': '08', 'week': '34', 'weekday': 'Saturday', 'close': '29168.7202'}, {'date': '2017-08-27', 'month': '08', 'week': '34', 'weekday': 'Sunday', 'close': '28945.352'}, {'date': '2017-08-28', 'month': '08', 'week': '35', 'weekday': 'Monday', 'close': '29340.152'}, {'date': '2017-08-29', 'month': '08', 'week': '35', 'weekday': 'Tuesday', 'close': '30656.5006'}, {'date': '2017-08-30', 'month': '08', 'week': '35', 'weekday': 'Wednesday', 'close': '30532.943'}, {'date': '2017-08-31', 'month': '08', 'week': '35', 'weekday': 'Thursday', 'close': '31391.9005'}, {'date': '2017-09-01', 'month': '09', 'week': '35', 'weekday': 'Friday', 'close': '32482.9375'}, {'date': '2017-09-02', 'month': '09', 'week': '35', 'weekday': 'Saturday', 'close': '30470.2819'}, {'date': '2017-09-03', 'month': '09', 'week': '35', 'weekday': 'Sunday', 'close': '30336.7'}, {'date': '2017-09-04', 'month': '09', 'week': '36', 'weekday': 'Monday', 'close': '28208.2898'}, {'date': '2017-09-05', 'month': '09', 'week': '36', 'weekday': 'Tuesday', 'close': '28918.0205'}, {'date': '2017-09-06', 'month': '09', 'week': '36', 'weekday': 'Wednesday', 'close': '30181.6445'}, {'date': '2017-09-07', 'month': '09', 'week': '36', 'weekday': 'Thursday', 'close': '30089.0416'}, {'date': '2017-09-08', 'month': '09', 'week': '36', 'weekday': 'Friday', 'close': '27976.5645'}, {'date': '2017-09-09', 'month': '09', 'week': '36', 'weekday': 'Saturday', 'close': '27818.6782'}, {'date': '2017-09-10', 'month': '09', 'week': '36', 'weekday': 'Sunday', 'close': '27359.9317'}, {'date': '2017-09-11', 'month': '09', 'week': '37', 'weekday': 'Monday', 'close': '27351.0555'}, {'date': '2017-09-12', 'month': '09', 'week': '37', 'weekday': 'Tuesday', 'close': '27111.147'}, {'date': '2017-09-13', 'month': '09', 'week': '37', 'weekday': 'Wednesday', 'close': '25354.506'}, {'date': '2017-09-14', 'month': '09', 'week': '37', 'weekday': 'Thursday', 'close': '21152.8443'}, {'date': '2017-09-15', 'month': '09', 'week': '37', 'weekday': 'Friday', 'close': '24164.8636'}, {'date': '2017-09-16', 'month': '09', 'week': '37', 'weekday': 'Saturday', 'close': '24111.3645'}, {'date': '2017-09-17', 'month': '09', 'week': '37', 'weekday': 'Sunday', 'close': '24057.8213'}, {'date': '2017-09-18', 'month': '09', 'week': '38', 'weekday': 'Monday', 'close': '26737.3742'}, {'date': '2017-09-19', 'month': '09', 'week': '38', 'weekday': 'Tuesday', 'close': '25652.4813'}, {'date': '2017-09-20', 'month': '09', 'week': '38', 'weekday': 'Wednesday', 'close': '25361.3238'}, {'date': '2017-09-21', 'month': '09', 'week': '38', 'weekday': 'Thursday', 'close': '23804.5608'}, {'date': '2017-09-22', 'month': '09', 'week': '38', 'weekday': 'Friday', 'close': '23761.1198'}, {'date': '2017-09-23', 'month': '09', 'week': '38', 'weekday': 'Saturday', 'close': '24908.4204'}, {'date': '2017-09-24', 'month': '09', 'week': '38', 'weekday': 'Sunday', 'close': '24216.5269'}, {'date': '2017-09-25', 'month': '09', 'week': '39', 'weekday': 'Monday', 'close': '26007.1112'}, {'date': '2017-09-26', 'month': '09', 'week': '39', 'weekday': 'Tuesday', 'close': '25869.3194'}, {'date': '2017-09-27', 'month': '09', 'week': '39', 'weekday': 'Wednesday', 'close': '27955.6252'}, {'date': '2017-09-28', 'month': '09', 'week': '39', 'weekday': 'Thursday', 'close': '27882.4195'}, {'date': '2017-09-29', 'month': '09', 'week': '39', 'weekday': 'Friday', 'close': '27711.6948'}, {'date': '2017-09-30', 'month': '09', 'week': '39', 'weekday': 'Saturday', 'close': '28969.0962'}, {'date': '2017-10-01', 'month': '10', 'week': '39', 'weekday': 'Sunday', 'close': '29264.4926'}, {'date': '2017-10-02', 'month': '10', 'week': '40', 'weekday': 'Monday', 'close': '29295.7562'}, {'date': '2017-10-03', 'month': '10', 'week': '40', 'weekday': 'Tuesday', 'close': '28743.0928'}, {'date': '2017-10-04', 'month': '10', 'week': '40', 'weekday': 'Wednesday', 'close': '28120.5656'}, {'date': '2017-10-05', 'month': '10', 'week': '40', 'weekday': 'Thursday', 'close': '28764.0436'}, {'date': '2017-10-06', 'month': '10', 'week': '40', 'weekday': 'Friday', 'close': '29084.1981'}, {'date': '2017-10-07', 'month': '10', 'week': '40', 'weekday': 'Saturday', 'close': '29521.3602'}, {'date': '2017-10-08', 'month': '10', 'week': '40', 'weekday': 'Sunday', 'close': '30583.2886'}, {'date': '2017-10-09', 'month': '10', 'week': '41', 'weekday': 'Monday', 'close': '31622.869'}, {'date': '2017-10-10', 'month': '10', 'week': '41', 'weekday': 'Tuesday', 'close': '31243.3645'}, {'date': '2017-10-11', 'month': '10', 'week': '41', 'weekday': 'Wednesday', 'close': '31830.4848'}, {'date': '2017-10-12', 'month': '10', 'week': '41', 'weekday': 'Thursday', 'close': '35833.2539'}, {'date': '2017-10-13', 'month': '10', 'week': '41', 'weekday': 'Friday', 'close': '37106.6814'}, {'date': '2017-10-14', 'month': '10', 'week': '41', 'weekday': 'Saturday', 'close': '38222.2666'}, {'date': '2017-10-15', 'month': '10', 'week': '41', 'weekday': 'Sunday', 'close': '37517.0856'}, {'date': '2017-10-16', 'month': '10', 'week': '42', 'weekday': 'Monday', 'close': '37917.9925'}, {'date': '2017-10-17', 'month': '10', 'week': '42', 'weekday': 'Tuesday', 'close': '37060.3182'}, {'date': '2017-10-18', 'month': '10', 'week': '42', 'weekday': 'Wednesday', 'close': '36928.632'}, {'date': '2017-10-19', 'month': '10', 'week': '42', 'weekday': 'Thursday', 'close': '37704.4579'}, {'date': '2017-10-20', 'month': '10', 'week': '42', 'weekday': 'Friday', 'close': '39634.3994'}, {'date': '2017-10-21', 'month': '10', 'week': '42', 'weekday': 'Saturday', 'close': '39827.4189'}, {'date': '2017-10-22', 'month': '10', 'week': '42', 'weekday': 'Sunday', 'close': '39673.3776'}, {'date': '2017-10-23', 'month': '10', 'week': '43', 'weekday': 'Monday', 'close': '39144.7834'}, {'date': '2017-10-24', 'month': '10', 'week': '43', 'weekday': 'Tuesday', 'close': '36611.829'}, {'date': '2017-10-25', 'month': '10', 'week': '43', 'weekday': 'Wednesday', 'close': '38067.9613'}, {'date': '2017-10-26', 'month': '10', 'week': '43', 'weekday': 'Thursday', 'close': '39108.6475'}, {'date': '2017-10-27', 'month': '10', 'week': '43', 'weekday': 'Friday', 'close': '38353.9173'}, {'date': '2017-10-28', 'month': '10', 'week': '43', 'weekday': 'Saturday', 'close': '38122.1385'}, {'date': '2017-10-29', 'month': '10', 'week': '43', 'weekday': 'Sunday', 'close': '40925.4142'}, {'date': '2017-10-30', 'month': '10', 'week': '44', 'weekday': 'Monday', 'close': '40682.7268'}, {'date': '2017-10-31', 'month': '10', 'week': '44', 'weekday': 'Tuesday', 'close': '42779.3067'}, {'date': '2017-11-01', 'month': '11', 'week': '44', 'weekday': 'Wednesday', 'close': '44572.0627'}, {'date': '2017-11-02', 'month': '11', 'week': '44', 'weekday': 'Thursday', 'close': '46462.0654'}, {'date': '2017-11-03', 'month': '11', 'week': '44', 'weekday': 'Friday', 'close': '47518.0205'}, {'date': '2017-11-04', 'month': '11', 'week': '44', 'weekday': 'Saturday', 'close': '49047.8253'}, {'date': '2017-11-05', 'month': '11', 'week': '44', 'weekday': 'Sunday', 'close': '48907.9843'}, {'date': '2017-11-06', 'month': '11', 'week': '45', 'weekday': 'Monday', 'close': '46159.7307'}, {'date': '2017-11-07', 'month': '11', 'week': '45', 'weekday': 'Tuesday', 'close': '47249.3415'}, {'date': '2017-11-08', 'month': '11', 'week': '45', 'weekday': 'Wednesday', 'close': '49427.7048'}, {'date': '2017-11-09', 'month': '11', 'week': '45', 'weekday': 'Thursday', 'close': '47448.9118'}, {'date': '2017-11-10', 'month': '11', 'week': '45', 'weekday': 'Friday', 'close': '43637.0596'}, {'date': '2017-11-11', 'month': '11', 'week': '45', 'weekday': 'Saturday', 'close': '42085.6019'}, {'date': '2017-11-12', 'month': '11', 'week': '45', 'weekday': 'Sunday', 'close': '38904.304'}, {'date': '2017-11-13', 'month': '11', 'week': '46', 'weekday': 'Monday', 'close': '43279.9771'}, {'date': '2017-11-14', 'month': '11', 'week': '46', 'weekday': 'Tuesday', 'close': '43801.9668'}, {'date': '2017-11-15', 'month': '11', 'week': '46', 'weekday': 'Wednesday', 'close': '48232.4671'}, {'date': '2017-11-16', 'month': '11', 'week': '46', 'weekday': 'Thursday', 'close': '52012.2859'}, {'date': '2017-11-17', 'month': '11', 'week': '46', 'weekday': 'Friday', 'close': '50973.7739'}, {'date': '2017-11-18', 'month': '11', 'week': '46', 'weekday': 'Saturday', 'close': '51542.0595'}, {'date': '2017-11-19', 'month': '11', 'week': '46', 'weekday': 'Sunday', 'close': '53279.4604'}, {'date': '2017-11-20', 'month': '11', 'week': '47', 'weekday': 'Monday', 'close': '54656.3545'}, {'date': '2017-11-21', 'month': '11', 'week': '47', 'weekday': 'Tuesday', 'close': '53673.7877'}, {'date': '2017-11-22', 'month': '11', 'week': '47', 'weekday': 'Wednesday', 'close': '54403.2305'}, {'date': '2017-11-23', 'month': '11', 'week': '47', 'weekday': 'Thursday', 'close': '52676.5845'}, {'date': '2017-11-24', 'month': '11', 'week': '47', 'weekday': 'Friday', 'close': '54136.6166'}, {'date': '2017-11-25', 'month': '11', 'week': '47', 'weekday': 'Saturday', 'close': '57851.0594'}, {'date': '2017-11-26', 'month': '11', 'week': '47', 'weekday': 'Sunday', 'close': '60980.0178'}, {'date': '2017-11-27', 'month': '11', 'week': '48', 'weekday': 'Monday', 'close': '64246.5971'}, {'date': '2017-11-28', 'month': '11', 'week': '48', 'weekday': 'Tuesday', 'close': '65458.8612'}, {'date': '2017-11-29', 'month': '11', 'week': '48', 'weekday': 'Wednesday', 'close': '64890.9642'}, {'date': '2017-11-30', 'month': '11', 'week': '48', 'weekday': 'Thursday', 'close': '65583.2597'}, {'date': '2017-12-01', 'month': '12', 'week': '48', 'weekday': 'Friday', 'close': '71825.6883'}, {'date': '2017-12-02', 'month': '12', 'week': '48', 'weekday': 'Saturday', 'close': '72079.2312'}, {'date': '2017-12-03', 'month': '12', 'week': '48', 'weekday': 'Sunday', 'close': '74007.4136'}, {'date': '2017-12-04', 'month': '12', 'week': '49', 'weekday': 'Monday', 'close': '76852.0129'}, {'date': '2017-12-05', 'month': '12', 'week': '49', 'weekday': 'Tuesday', 'close': '77398.8752'}, {'date': '2017-12-06', 'month': '12', 'week': '49', 'weekday': 'Wednesday', 'close': '90679.5487'}, {'date': '2017-12-07', 'month': '12', 'week': '49', 'weekday': 'Thursday', 'close': '111589.9776'}, {'date': '2017-12-08', 'month': '12', 'week': '49', 'weekday': 'Friday', 'close': '106233.201'}, {'date': '2017-12-09', 'month': '12', 'week': '49', 'weekday': 'Saturday', 'close': '98676.7747'}, {'date': '2017-12-10', 'month': '12', 'week': '49', 'weekday': 'Sunday', 'close': '99525.1027'}, {'date': '2017-12-11', 'month': '12', 'week': '50', 'weekday': 'Monday', 'close': '110642.88'}, {'date': '2017-12-12', 'month': '12', 'week': '50', 'weekday': 'Tuesday', 'close': '113732.6745'}]
    


```python
print(type(req.text), type(req.json()))
```

    <class 'str'> <class 'list'>
    


```python
#读取json到一个列表中：(注意json中每个键和值都是str)
filename = 'btc_close_2017.json'
with open(filename) as f:
    btc_data = json.load(f)

for btc_dict in btc_data:
    date = btc_dict['date']
    month = int(btc_dict['month'])
    week = int(btc_dict['week'])
    weekday = btc_dict['weekday']
    # 注意python直接将‘6928.6492’小数字符串转为整数，所以要先float()
    close = int(float(btc_dict['close']))   
    
    print("{} is month {} week {}, {}, the close price is {} RMB".format(
        date, month, week, weekday, close))
    
```

    2017-01-01 is month 1 week 52, Sunday, the close price is 6928 RMB
    2017-01-02 is month 1 week 1, Monday, the close price is 7070 RMB
    2017-01-03 is month 1 week 1, Tuesday, the close price is 7175 RMB
    2017-01-04 is month 1 week 1, Wednesday, the close price is 7835 RMB
    2017-01-05 is month 1 week 1, Thursday, the close price is 6928 RMB
    2017-01-06 is month 1 week 1, Friday, the close price is 6196 RMB
    2017-01-07 is month 1 week 1, Saturday, the close price is 6262 RMB
    2017-01-08 is month 1 week 1, Sunday, the close price is 6319 RMB
    2017-01-09 is month 1 week 2, Monday, the close price is 6239 RMB
    2017-01-10 is month 1 week 2, Tuesday, the close price is 6263 RMB
    2017-01-11 is month 1 week 2, Wednesday, the close price is 5383 RMB
    2017-01-12 is month 1 week 2, Thursday, the close price is 5566 RMB
    2017-01-13 is month 1 week 2, Friday, the close price is 5700 RMB
    2017-01-14 is month 1 week 2, Saturday, the close price is 5648 RMB
    2017-01-15 is month 1 week 2, Sunday, the close price is 5674 RMB
    2017-01-16 is month 1 week 3, Monday, the close price is 5730 RMB
    2017-01-17 is month 1 week 3, Tuesday, the close price is 6202 RMB
    2017-01-18 is month 1 week 3, Wednesday, the close price is 6047 RMB
    2017-01-19 is month 1 week 3, Thursday, the close price is 6170 RMB
    2017-01-20 is month 1 week 3, Friday, the close price is 6131 RMB
    2017-01-21 is month 1 week 3, Saturday, the close price is 6326 RMB
    2017-01-22 is month 1 week 3, Sunday, the close price is 6362 RMB
    2017-01-23 is month 1 week 4, Monday, the close price is 6255 RMB
    2017-01-24 is month 1 week 4, Tuesday, the close price is 6074 RMB
    2017-01-25 is month 1 week 4, Wednesday, the close price is 6154 RMB
    2017-01-26 is month 1 week 4, Thursday, the close price is 6295 RMB
    2017-01-27 is month 1 week 4, Friday, the close price is 6320 RMB
    2017-01-28 is month 1 week 4, Saturday, the close price is 6332 RMB
    2017-01-29 is month 1 week 4, Sunday, the close price is 6289 RMB
    2017-01-30 is month 1 week 5, Monday, the close price is 6332 RMB
    2017-01-31 is month 1 week 5, Tuesday, the close price is 6657 RMB
    2017-02-01 is month 2 week 5, Wednesday, the close price is 6793 RMB
    2017-02-02 is month 2 week 5, Thursday, the close price is 6934 RMB
    2017-02-03 is month 2 week 5, Friday, the close price is 6995 RMB
    2017-02-04 is month 2 week 5, Saturday, the close price is 7102 RMB
    2017-02-05 is month 2 week 5, Sunday, the close price is 6965 RMB
    2017-02-06 is month 2 week 6, Monday, the close price is 7034 RMB
    2017-02-07 is month 2 week 6, Tuesday, the close price is 7245 RMB
    2017-02-08 is month 2 week 6, Wednesday, the close price is 7246 RMB
    2017-02-09 is month 2 week 6, Thursday, the close price is 6811 RMB
    2017-02-10 is month 2 week 6, Friday, the close price is 6833 RMB
    2017-02-11 is month 2 week 6, Saturday, the close price is 6946 RMB
    2017-02-12 is month 2 week 6, Sunday, the close price is 6883 RMB
    2017-02-13 is month 2 week 7, Monday, the close price is 6858 RMB
    2017-02-14 is month 2 week 7, Tuesday, the close price is 6930 RMB
    2017-02-15 is month 2 week 7, Wednesday, the close price is 6935 RMB
    2017-02-16 is month 2 week 7, Thursday, the close price is 7088 RMB
    2017-02-17 is month 2 week 7, Friday, the close price is 7229 RMB
    2017-02-18 is month 2 week 7, Saturday, the close price is 7267 RMB
    2017-02-19 is month 2 week 7, Sunday, the close price is 7220 RMB
    2017-02-20 is month 2 week 8, Monday, the close price is 7450 RMB
    2017-02-21 is month 2 week 8, Tuesday, the close price is 7732 RMB
    2017-02-22 is month 2 week 8, Wednesday, the close price is 7716 RMB
    2017-02-23 is month 2 week 8, Thursday, the close price is 8092 RMB
    2017-02-24 is month 2 week 8, Friday, the close price is 8109 RMB
    2017-02-25 is month 2 week 8, Saturday, the close price is 7908 RMB
    2017-02-26 is month 2 week 8, Sunday, the close price is 8137 RMB
    2017-02-27 is month 2 week 9, Monday, the close price is 8206 RMB
    2017-02-28 is month 2 week 9, Tuesday, the close price is 8176 RMB
    2017-03-01 is month 3 week 9, Wednesday, the close price is 8464 RMB
    2017-03-02 is month 3 week 9, Thursday, the close price is 8688 RMB
    2017-03-03 is month 3 week 9, Friday, the close price is 8900 RMB
    2017-03-04 is month 3 week 9, Saturday, the close price is 8741 RMB
    2017-03-05 is month 3 week 9, Sunday, the close price is 8816 RMB
    2017-03-06 is month 3 week 10, Monday, the close price is 8832 RMB
    2017-03-07 is month 3 week 10, Tuesday, the close price is 8504 RMB
    2017-03-08 is month 3 week 10, Wednesday, the close price is 7953 RMB
    2017-03-09 is month 3 week 10, Thursday, the close price is 8235 RMB
    2017-03-10 is month 3 week 10, Friday, the close price is 7716 RMB
    2017-03-11 is month 3 week 10, Saturday, the close price is 8161 RMB
    2017-03-12 is month 3 week 10, Sunday, the close price is 8441 RMB
    2017-03-13 is month 3 week 11, Monday, the close price is 8595 RMB
    2017-03-14 is month 3 week 11, Tuesday, the close price is 8616 RMB
    2017-03-15 is month 3 week 11, Wednesday, the close price is 8711 RMB
    2017-03-16 is month 3 week 11, Thursday, the close price is 8091 RMB
    2017-03-17 is month 3 week 11, Friday, the close price is 7379 RMB
    2017-03-18 is month 3 week 11, Saturday, the close price is 6694 RMB
    2017-03-19 is month 3 week 11, Sunday, the close price is 7028 RMB
    2017-03-20 is month 3 week 12, Monday, the close price is 7196 RMB
    2017-03-21 is month 3 week 12, Tuesday, the close price is 7680 RMB
    2017-03-22 is month 3 week 12, Wednesday, the close price is 7139 RMB
    2017-03-23 is month 3 week 12, Thursday, the close price is 7092 RMB
    2017-03-24 is month 3 week 12, Friday, the close price is 6437 RMB
    2017-03-25 is month 3 week 12, Saturday, the close price is 6640 RMB
    2017-03-26 is month 3 week 12, Sunday, the close price is 6623 RMB
    2017-03-27 is month 3 week 13, Monday, the close price is 7151 RMB
    2017-03-28 is month 3 week 13, Tuesday, the close price is 7184 RMB
    2017-03-29 is month 3 week 13, Wednesday, the close price is 7168 RMB
    2017-03-30 is month 3 week 13, Thursday, the close price is 7146 RMB
    2017-03-31 is month 3 week 13, Friday, the close price is 7439 RMB
    2017-04-01 is month 4 week 13, Saturday, the close price is 7506 RMB
    2017-04-02 is month 4 week 13, Sunday, the close price is 7566 RMB
    2017-04-03 is month 4 week 14, Monday, the close price is 7903 RMB
    2017-04-04 is month 4 week 14, Tuesday, the close price is 7874 RMB
    2017-04-05 is month 4 week 14, Wednesday, the close price is 7827 RMB
    2017-04-06 is month 4 week 14, Thursday, the close price is 8212 RMB
    2017-04-07 is month 4 week 14, Friday, the close price is 8236 RMB
    2017-04-08 is month 4 week 14, Saturday, the close price is 8180 RMB
    2017-04-09 is month 4 week 14, Sunday, the close price is 8354 RMB
    2017-04-10 is month 4 week 15, Monday, the close price is 8375 RMB
    2017-04-11 is month 4 week 15, Tuesday, the close price is 8442 RMB
    2017-04-12 is month 4 week 15, Wednesday, the close price is 8382 RMB
    2017-04-13 is month 4 week 15, Thursday, the close price is 8117 RMB
    2017-04-14 is month 4 week 15, Friday, the close price is 8151 RMB
    2017-04-15 is month 4 week 15, Saturday, the close price is 8129 RMB
    2017-04-16 is month 4 week 15, Sunday, the close price is 8167 RMB
    2017-04-17 is month 4 week 16, Monday, the close price is 8267 RMB
    2017-04-18 is month 4 week 16, Tuesday, the close price is 8379 RMB
    2017-04-19 is month 4 week 16, Wednesday, the close price is 8436 RMB
    2017-04-20 is month 4 week 16, Thursday, the close price is 8639 RMB
    2017-04-21 is month 4 week 16, Friday, the close price is 8654 RMB
    2017-04-22 is month 4 week 16, Saturday, the close price is 8567 RMB
    2017-04-23 is month 4 week 16, Sunday, the close price is 8458 RMB
    2017-04-24 is month 4 week 17, Monday, the close price is 8594 RMB
    2017-04-25 is month 4 week 17, Tuesday, the close price is 8700 RMB
    2017-04-26 is month 4 week 17, Wednesday, the close price is 8857 RMB
    2017-04-27 is month 4 week 17, Thursday, the close price is 9167 RMB
    2017-04-28 is month 4 week 17, Friday, the close price is 9101 RMB
    2017-04-29 is month 4 week 17, Saturday, the close price is 9149 RMB
    2017-04-30 is month 4 week 17, Sunday, the close price is 9325 RMB
    2017-05-01 is month 5 week 18, Monday, the close price is 9665 RMB
    2017-05-02 is month 5 week 18, Tuesday, the close price is 9944 RMB
    2017-05-03 is month 5 week 18, Wednesday, the close price is 10292 RMB
    2017-05-04 is month 5 week 18, Thursday, the close price is 10452 RMB
    2017-05-05 is month 5 week 18, Friday, the close price is 10439 RMB
    2017-05-06 is month 5 week 18, Saturday, the close price is 10688 RMB
    2017-05-07 is month 5 week 18, Sunday, the close price is 10660 RMB
    2017-05-08 is month 5 week 19, Monday, the close price is 11317 RMB
    2017-05-09 is month 5 week 19, Tuesday, the close price is 11794 RMB
    2017-05-10 is month 5 week 19, Wednesday, the close price is 12126 RMB
    2017-05-11 is month 5 week 19, Thursday, the close price is 12478 RMB
    2017-05-12 is month 5 week 19, Friday, the close price is 11569 RMB
    2017-05-13 is month 5 week 19, Saturday, the close price is 12141 RMB
    2017-05-14 is month 5 week 19, Sunday, the close price is 12229 RMB
    2017-05-15 is month 5 week 20, Monday, the close price is 11701 RMB
    2017-05-16 is month 5 week 20, Tuesday, the close price is 11835 RMB
    2017-05-17 is month 5 week 20, Wednesday, the close price is 12403 RMB
    2017-05-18 is month 5 week 20, Thursday, the close price is 13002 RMB
    2017-05-19 is month 5 week 20, Friday, the close price is 13549 RMB
    2017-05-20 is month 5 week 20, Saturday, the close price is 14127 RMB
    2017-05-21 is month 5 week 20, Sunday, the close price is 14091 RMB
    2017-05-22 is month 5 week 21, Monday, the close price is 14731 RMB
    2017-05-23 is month 5 week 21, Tuesday, the close price is 15784 RMB
    2017-05-24 is month 5 week 21, Wednesday, the close price is 17061 RMB
    2017-05-25 is month 5 week 21, Thursday, the close price is 16190 RMB
    2017-05-26 is month 5 week 21, Friday, the close price is 15402 RMB
    2017-05-27 is month 5 week 21, Saturday, the close price is 14440 RMB
    2017-05-28 is month 5 week 21, Sunday, the close price is 15139 RMB
    2017-05-29 is month 5 week 22, Monday, the close price is 15700 RMB
    2017-05-30 is month 5 week 22, Tuesday, the close price is 15064 RMB
    2017-05-31 is month 5 week 22, Wednesday, the close price is 15869 RMB
    2017-06-01 is month 6 week 22, Thursday, the close price is 16693 RMB
    2017-06-02 is month 6 week 22, Friday, the close price is 17149 RMB
    2017-06-03 is month 6 week 22, Saturday, the close price is 17410 RMB
    2017-06-04 is month 6 week 22, Sunday, the close price is 17399 RMB
    2017-06-05 is month 6 week 23, Monday, the close price is 18621 RMB
    2017-06-06 is month 6 week 23, Tuesday, the close price is 19797 RMB
    2017-06-07 is month 6 week 23, Wednesday, the close price is 18205 RMB
    2017-06-08 is month 6 week 23, Thursday, the close price is 19209 RMB
    2017-06-09 is month 6 week 23, Friday, the close price is 19218 RMB
    2017-06-10 is month 6 week 23, Saturday, the close price is 20004 RMB
    2017-06-11 is month 6 week 23, Sunday, the close price is 20472 RMB
    2017-06-12 is month 6 week 24, Monday, the close price is 18234 RMB
    2017-06-13 is month 6 week 24, Tuesday, the close price is 18615 RMB
    2017-06-14 is month 6 week 24, Wednesday, the close price is 16946 RMB
    2017-06-15 is month 6 week 24, Thursday, the close price is 16724 RMB
    2017-06-16 is month 6 week 24, Friday, the close price is 17217 RMB
    2017-06-17 is month 6 week 24, Saturday, the close price is 18142 RMB
    2017-06-18 is month 6 week 24, Sunday, the close price is 17535 RMB
    2017-06-19 is month 6 week 25, Monday, the close price is 18015 RMB
    2017-06-20 is month 6 week 25, Tuesday, the close price is 18975 RMB
    2017-06-21 is month 6 week 25, Wednesday, the close price is 18522 RMB
    2017-06-22 is month 6 week 25, Thursday, the close price is 18733 RMB
    2017-06-23 is month 6 week 25, Friday, the close price is 18720 RMB
    2017-06-24 is month 6 week 25, Saturday, the close price is 17906 RMB
    2017-06-25 is month 6 week 25, Sunday, the close price is 17734 RMB
    2017-06-26 is month 6 week 26, Monday, the close price is 17001 RMB
    2017-06-27 is month 6 week 26, Tuesday, the close price is 17666 RMB
    2017-06-28 is month 6 week 26, Wednesday, the close price is 17575 RMB
    2017-06-29 is month 6 week 26, Thursday, the close price is 17385 RMB
    2017-06-30 is month 6 week 26, Friday, the close price is 16943 RMB
    2017-07-01 is month 7 week 26, Saturday, the close price is 16674 RMB
    2017-07-02 is month 7 week 26, Sunday, the close price is 17150 RMB
    2017-07-03 is month 7 week 27, Monday, the close price is 17549 RMB
    2017-07-04 is month 7 week 27, Tuesday, the close price is 17851 RMB
    2017-07-05 is month 7 week 27, Wednesday, the close price is 17812 RMB
    2017-07-06 is month 7 week 27, Thursday, the close price is 17813 RMB
    2017-07-07 is month 7 week 27, Friday, the close price is 17156 RMB
    2017-07-08 is month 7 week 27, Saturday, the close price is 17557 RMB
    2017-07-09 is month 7 week 27, Sunday, the close price is 17189 RMB
    2017-07-10 is month 7 week 28, Monday, the close price is 16137 RMB
    2017-07-11 is month 7 week 28, Tuesday, the close price is 15865 RMB
    2017-07-12 is month 7 week 28, Wednesday, the close price is 16446 RMB
    2017-07-13 is month 7 week 28, Thursday, the close price is 16036 RMB
    2017-07-14 is month 7 week 28, Friday, the close price is 15132 RMB
    2017-07-15 is month 7 week 28, Saturday, the close price is 13510 RMB
    2017-07-16 is month 7 week 28, Sunday, the close price is 13075 RMB
    2017-07-17 is month 7 week 29, Monday, the close price is 15192 RMB
    2017-07-18 is month 7 week 29, Tuesday, the close price is 15706 RMB
    2017-07-19 is month 7 week 29, Wednesday, the close price is 15491 RMB
    2017-07-20 is month 7 week 29, Thursday, the close price is 19449 RMB
    2017-07-21 is month 7 week 29, Friday, the close price is 18231 RMB
    2017-07-22 is month 7 week 29, Saturday, the close price is 19210 RMB
    2017-07-23 is month 7 week 29, Sunday, the close price is 18585 RMB
    2017-07-24 is month 7 week 30, Monday, the close price is 18762 RMB
    2017-07-25 is month 7 week 30, Tuesday, the close price is 17489 RMB
    2017-07-26 is month 7 week 30, Wednesday, the close price is 17219 RMB
    2017-07-27 is month 7 week 30, Thursday, the close price is 18188 RMB
    2017-07-28 is month 7 week 30, Friday, the close price is 18898 RMB
    2017-07-29 is month 7 week 30, Saturday, the close price is 18326 RMB
    2017-07-30 is month 7 week 30, Sunday, the close price is 18499 RMB
    2017-07-31 is month 7 week 31, Monday, the close price is 19334 RMB
    2017-08-01 is month 8 week 31, Tuesday, the close price is 18376 RMB
    2017-08-02 is month 8 week 31, Wednesday, the close price is 18305 RMB
    2017-08-03 is month 8 week 31, Thursday, the close price is 18901 RMB
    2017-08-04 is month 8 week 31, Friday, the close price is 19412 RMB
    2017-08-05 is month 8 week 31, Saturday, the close price is 22227 RMB
    2017-08-06 is month 8 week 31, Sunday, the close price is 22027 RMB
    2017-08-07 is month 8 week 32, Monday, the close price is 23063 RMB
    2017-08-08 is month 8 week 32, Tuesday, the close price is 23332 RMB
    2017-08-09 is month 8 week 32, Wednesday, the close price is 22541 RMB
    2017-08-10 is month 8 week 32, Thursday, the close price is 22904 RMB
    2017-08-11 is month 8 week 32, Friday, the close price is 24526 RMB
    2017-08-12 is month 8 week 32, Saturday, the close price is 26109 RMB
    2017-08-13 is month 8 week 32, Sunday, the close price is 27390 RMB
    2017-08-14 is month 8 week 33, Monday, the close price is 29237 RMB
    2017-08-15 is month 8 week 33, Tuesday, the close price is 28073 RMB
    2017-08-16 is month 8 week 33, Wednesday, the close price is 29612 RMB
    2017-08-17 is month 8 week 33, Thursday, the close price is 28816 RMB
    2017-08-18 is month 8 week 33, Friday, the close price is 27752 RMB
    2017-08-19 is month 8 week 33, Saturday, the close price is 28062 RMB
    2017-08-20 is month 8 week 33, Sunday, the close price is 27416 RMB
    2017-08-21 is month 8 week 34, Monday, the close price is 26919 RMB
    2017-08-22 is month 8 week 34, Tuesday, the close price is 27565 RMB
    2017-08-23 is month 8 week 34, Wednesday, the close price is 27907 RMB
    2017-08-24 is month 8 week 34, Thursday, the close price is 29063 RMB
    2017-08-25 is month 8 week 34, Friday, the close price is 29305 RMB
    2017-08-26 is month 8 week 34, Saturday, the close price is 29168 RMB
    2017-08-27 is month 8 week 34, Sunday, the close price is 28945 RMB
    2017-08-28 is month 8 week 35, Monday, the close price is 29340 RMB
    2017-08-29 is month 8 week 35, Tuesday, the close price is 30656 RMB
    2017-08-30 is month 8 week 35, Wednesday, the close price is 30532 RMB
    2017-08-31 is month 8 week 35, Thursday, the close price is 31391 RMB
    2017-09-01 is month 9 week 35, Friday, the close price is 32482 RMB
    2017-09-02 is month 9 week 35, Saturday, the close price is 30470 RMB
    2017-09-03 is month 9 week 35, Sunday, the close price is 30336 RMB
    2017-09-04 is month 9 week 36, Monday, the close price is 28208 RMB
    2017-09-05 is month 9 week 36, Tuesday, the close price is 28918 RMB
    2017-09-06 is month 9 week 36, Wednesday, the close price is 30181 RMB
    2017-09-07 is month 9 week 36, Thursday, the close price is 30089 RMB
    2017-09-08 is month 9 week 36, Friday, the close price is 27976 RMB
    2017-09-09 is month 9 week 36, Saturday, the close price is 27818 RMB
    2017-09-10 is month 9 week 36, Sunday, the close price is 27359 RMB
    2017-09-11 is month 9 week 37, Monday, the close price is 27351 RMB
    2017-09-12 is month 9 week 37, Tuesday, the close price is 27111 RMB
    2017-09-13 is month 9 week 37, Wednesday, the close price is 25354 RMB
    2017-09-14 is month 9 week 37, Thursday, the close price is 21152 RMB
    2017-09-15 is month 9 week 37, Friday, the close price is 24164 RMB
    2017-09-16 is month 9 week 37, Saturday, the close price is 24111 RMB
    2017-09-17 is month 9 week 37, Sunday, the close price is 24057 RMB
    2017-09-18 is month 9 week 38, Monday, the close price is 26737 RMB
    2017-09-19 is month 9 week 38, Tuesday, the close price is 25652 RMB
    2017-09-20 is month 9 week 38, Wednesday, the close price is 25361 RMB
    2017-09-21 is month 9 week 38, Thursday, the close price is 23804 RMB
    2017-09-22 is month 9 week 38, Friday, the close price is 23761 RMB
    2017-09-23 is month 9 week 38, Saturday, the close price is 24908 RMB
    2017-09-24 is month 9 week 38, Sunday, the close price is 24216 RMB
    2017-09-25 is month 9 week 39, Monday, the close price is 26007 RMB
    2017-09-26 is month 9 week 39, Tuesday, the close price is 25869 RMB
    2017-09-27 is month 9 week 39, Wednesday, the close price is 27955 RMB
    2017-09-28 is month 9 week 39, Thursday, the close price is 27882 RMB
    2017-09-29 is month 9 week 39, Friday, the close price is 27711 RMB
    2017-09-30 is month 9 week 39, Saturday, the close price is 28969 RMB
    2017-10-01 is month 10 week 39, Sunday, the close price is 29264 RMB
    2017-10-02 is month 10 week 40, Monday, the close price is 29295 RMB
    2017-10-03 is month 10 week 40, Tuesday, the close price is 28743 RMB
    2017-10-04 is month 10 week 40, Wednesday, the close price is 28120 RMB
    2017-10-05 is month 10 week 40, Thursday, the close price is 28764 RMB
    2017-10-06 is month 10 week 40, Friday, the close price is 29084 RMB
    2017-10-07 is month 10 week 40, Saturday, the close price is 29521 RMB
    2017-10-08 is month 10 week 40, Sunday, the close price is 30583 RMB
    2017-10-09 is month 10 week 41, Monday, the close price is 31622 RMB
    2017-10-10 is month 10 week 41, Tuesday, the close price is 31243 RMB
    2017-10-11 is month 10 week 41, Wednesday, the close price is 31830 RMB
    2017-10-12 is month 10 week 41, Thursday, the close price is 35833 RMB
    2017-10-13 is month 10 week 41, Friday, the close price is 37106 RMB
    2017-10-14 is month 10 week 41, Saturday, the close price is 38222 RMB
    2017-10-15 is month 10 week 41, Sunday, the close price is 37517 RMB
    2017-10-16 is month 10 week 42, Monday, the close price is 37917 RMB
    2017-10-17 is month 10 week 42, Tuesday, the close price is 37060 RMB
    2017-10-18 is month 10 week 42, Wednesday, the close price is 36928 RMB
    2017-10-19 is month 10 week 42, Thursday, the close price is 37704 RMB
    2017-10-20 is month 10 week 42, Friday, the close price is 39634 RMB
    2017-10-21 is month 10 week 42, Saturday, the close price is 39827 RMB
    2017-10-22 is month 10 week 42, Sunday, the close price is 39673 RMB
    2017-10-23 is month 10 week 43, Monday, the close price is 39144 RMB
    2017-10-24 is month 10 week 43, Tuesday, the close price is 36611 RMB
    2017-10-25 is month 10 week 43, Wednesday, the close price is 38067 RMB
    2017-10-26 is month 10 week 43, Thursday, the close price is 39108 RMB
    2017-10-27 is month 10 week 43, Friday, the close price is 38353 RMB
    2017-10-28 is month 10 week 43, Saturday, the close price is 38122 RMB
    2017-10-29 is month 10 week 43, Sunday, the close price is 40925 RMB
    2017-10-30 is month 10 week 44, Monday, the close price is 40682 RMB
    2017-10-31 is month 10 week 44, Tuesday, the close price is 42779 RMB
    2017-11-01 is month 11 week 44, Wednesday, the close price is 44572 RMB
    2017-11-02 is month 11 week 44, Thursday, the close price is 46462 RMB
    2017-11-03 is month 11 week 44, Friday, the close price is 47518 RMB
    2017-11-04 is month 11 week 44, Saturday, the close price is 49047 RMB
    2017-11-05 is month 11 week 44, Sunday, the close price is 48907 RMB
    2017-11-06 is month 11 week 45, Monday, the close price is 46159 RMB
    2017-11-07 is month 11 week 45, Tuesday, the close price is 47249 RMB
    2017-11-08 is month 11 week 45, Wednesday, the close price is 49427 RMB
    2017-11-09 is month 11 week 45, Thursday, the close price is 47448 RMB
    2017-11-10 is month 11 week 45, Friday, the close price is 43637 RMB
    2017-11-11 is month 11 week 45, Saturday, the close price is 42085 RMB
    2017-11-12 is month 11 week 45, Sunday, the close price is 38904 RMB
    2017-11-13 is month 11 week 46, Monday, the close price is 43279 RMB
    2017-11-14 is month 11 week 46, Tuesday, the close price is 43801 RMB
    2017-11-15 is month 11 week 46, Wednesday, the close price is 48232 RMB
    2017-11-16 is month 11 week 46, Thursday, the close price is 52012 RMB
    2017-11-17 is month 11 week 46, Friday, the close price is 50973 RMB
    2017-11-18 is month 11 week 46, Saturday, the close price is 51542 RMB
    2017-11-19 is month 11 week 46, Sunday, the close price is 53279 RMB
    2017-11-20 is month 11 week 47, Monday, the close price is 54656 RMB
    2017-11-21 is month 11 week 47, Tuesday, the close price is 53673 RMB
    2017-11-22 is month 11 week 47, Wednesday, the close price is 54403 RMB
    2017-11-23 is month 11 week 47, Thursday, the close price is 52676 RMB
    2017-11-24 is month 11 week 47, Friday, the close price is 54136 RMB
    2017-11-25 is month 11 week 47, Saturday, the close price is 57851 RMB
    2017-11-26 is month 11 week 47, Sunday, the close price is 60980 RMB
    2017-11-27 is month 11 week 48, Monday, the close price is 64246 RMB
    2017-11-28 is month 11 week 48, Tuesday, the close price is 65458 RMB
    2017-11-29 is month 11 week 48, Wednesday, the close price is 64890 RMB
    2017-11-30 is month 11 week 48, Thursday, the close price is 65583 RMB
    2017-12-01 is month 12 week 48, Friday, the close price is 71825 RMB
    2017-12-02 is month 12 week 48, Saturday, the close price is 72079 RMB
    2017-12-03 is month 12 week 48, Sunday, the close price is 74007 RMB
    2017-12-04 is month 12 week 49, Monday, the close price is 76852 RMB
    2017-12-05 is month 12 week 49, Tuesday, the close price is 77398 RMB
    2017-12-06 is month 12 week 49, Wednesday, the close price is 90679 RMB
    2017-12-07 is month 12 week 49, Thursday, the close price is 111589 RMB
    2017-12-08 is month 12 week 49, Friday, the close price is 106233 RMB
    2017-12-09 is month 12 week 49, Saturday, the close price is 98676 RMB
    2017-12-10 is month 12 week 49, Sunday, the close price is 99525 RMB
    2017-12-11 is month 12 week 50, Monday, the close price is 110642 RMB
    2017-12-12 is month 12 week 50, Tuesday, the close price is 113732 RMB
    

######  用pygal 可视化功能


```python
filename = 'btc_close_2017.json'
with open(filename) as f:
    btc_data = json.load(f)
# 创建5个列表，分别存储日期和收盘价
dates = []
months = []
weeks = []
weekdays = []
close = []
# 每一天的信息
for btc_dict in btc_data:
    dates.append(btc_dict['date'])
    months.append(int(btc_dict['month']))
    weeks.append(int(btc_dict['week']))
    weekdays.append(btc_dict['weekday'])
    close.append(int(float(btc_dict['close'])))


```


```python
    
line_chart = pygal.Line(x_label_rotation=20, show_minor_x_labels=False)  
line_chart.title = '收盘价（¥）'
line_chart.x_labels = dates
N = 20  # x轴坐标每隔20天显示一次
line_chart.x_labels_major = dates[::N]  # ②
line_chart.add('收盘价', close)
line_chart.render_to_file('收盘价折线图（¥）.svg')
line_chart


line_chart = pygal.Line(x_label_rotation=20, show_minor_x_labels=False)
line_chart.title = '收盘价对数变换（¥）'
line_chart.x_labels = dates
N = 20  # x轴坐标每隔20天显示一次
line_chart.x_labels_major = dates[::N]
close_log = [math.log10(_) for _ in close]  
line_chart.add('log收盘价', close_log)
line_chart.render_to_file('收盘价对数变换折线图（¥）.svg')
line_chart

```


    ---------------------------------------------------------------------------

    OSError                                   Traceback (most recent call last)

    E:\Anaconda\lib\site-packages\IPython\core\formatters.py in __call__(self, obj)
        343             method = get_real_method(obj, self.print_method)
        344             if method is not None:
    --> 345                 return method()
        346             return None
        347         else:
    

    E:\Anaconda\lib\site-packages\pygal\graph\base.py in _repr_png_(self)
        232     def _repr_png_(self):
        233         """Display png in IPython notebook"""
    --> 234         return self.render_to_png()
    

    E:\Anaconda\lib\site-packages\pygal\graph\public.py in render_to_png(self, filename, dpi, **kwargs)
        116     def render_to_png(self, filename=None, dpi=72, **kwargs):
        117         """Render the graph, convert it to png and write it to filename"""
    --> 118         import cairosvg
        119         return cairosvg.svg2png(
        120             bytestring=self.render(**kwargs), write_to=filename, dpi=dpi)
    

    E:\Anaconda\lib\site-packages\cairosvg\__init__.py in <module>()
         39 
         40 # VERSION is used in the "url" module imported by "surface"
    ---> 41 from . import surface  # noqa
         42 
         43 
    

    E:\Anaconda\lib\site-packages\cairosvg\surface.py in <module>()
         22 import io
         23 
    ---> 24 import cairocffi as cairo
         25 
         26 from .colors import color
    

    E:\Anaconda\lib\site-packages\cairocffi\__init__.py in <module>()
         39 
         40 
    ---> 41 cairo = dlopen(ffi, 'cairo', 'cairo-2', 'cairo-gobject-2')
         42 
         43 
    

    E:\Anaconda\lib\site-packages\cairocffi\__init__.py in dlopen(ffi, *names)
         36             except OSError:
         37                 pass
    ---> 38     raise OSError("dlopen() failed to load a library: %s" % ' / '.join(names))
         39 
         40 
    

    OSError: dlopen() failed to load a library: cairo / cairo-2 / cairo-gobject-2





![svg](output_11_1.svg)

