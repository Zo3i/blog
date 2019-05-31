- **快速去重:**
```javascript
Array.from(new Set(arr));
```
![](https://zxx.im/wp-content/uploads/2018/12/YUGUEVBMLIKOXWWNLWAA.png)

---
- **数组求和:**
```javascript
1.eval(arr.join('+'))
```
![](https://zxx.im/wp-content/uploads/2018/12/@LASPB5B_R_7.png)
```javascript
2.arr.reduce((pre, cur) => pre + cur)
```
![](https://zxx.im/wp-content/uploads/2018/12/RJJM3ZE1_SJ1J_PXG.png)
---
- **稳定排序:**
```javascript
arr.sort((a, b) => a - b)//从小到大
arr.sort((a, b) => b - a)//从大到小
```
![](https://zxx.im/wp-content/uploads/2018/12/BVNJ_2TSAT_0TU.png)
![](https://zxx.im/wp-content/uploads/2018/12/RBVEVDH5W@75JSQC82.png)

---
- **修复精度错误:**
```javascript
var x = 0.9 - 0.8
console.log(x)
console.log(x.toFixed(2))
```
![](https://zxx.im/wp-content/uploads/2018/12/WVP4GH_QD3HR.png)
---
- **进制转换:**
```javascript
//1.十进制转其他
var x = 10
console.log(x.toString(8))
console.log(x.toString(32))
console.log(x.toString(16))
```
![](https://zxx.im/wp-content/uploads/2018/12/DAPKFO9XZY4D6YAJM4.png)

```javascript
//1.其他转十进制
var x = 100
console.log(parseInt(x, 2))
console.log(parseInt(x, 8))
console.log(parseInt(x, 16))
```
![](https://zxx.im/wp-content/uploads/2018/12/F21@4917M7PCLT.png)

- **数组展开**
```javascript
var arr = [1, 2, 3, 4, 5]
console.log(...arr)
```
![](https://zxx.im/wp-content/uploads/2018/12/CS38T3SWAOP2P@VJVR.png)

- **数组中最大数**
```javascript
var arr = [1, 2, 3, 4, 5]
console.log(Math.max(arr))
```
![](https://zxx.im/wp-content/uploads/2018/12/CA8U2QL_RPE9GSZ_9C8I.png)

- **同理数组中最小数**
```javascript
var arr = [1, 2, 3, 4, 5]
console.log(Math.min(...arr))
```
![](https://zxx.im/wp-content/uploads/2018/12/TYL60N8U4@1_655GAAYU.png)


