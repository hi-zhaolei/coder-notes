# React

## 生命周期

### componentDidMount

*tips* **componentDidMount**会等待**同级component**都执行完**render**方法之后，再按照**子父**,**前后**顺序依次执行，执行顺序与**render**一致。

### componentWillUnmount

*tips* **componentWillUnmount**会按照**父子**,**前后**顺序依次执行，执行顺序与**render**相反。
