---
order: 1
title: 表单中的应用示例
---

表单集合. (使用 [rc-form 文档](https://github.com/react-component/form) 做表单验证)

````__react
import { List, InputItem, Switch, Stepper, Slider, Button } from 'antd-mobile';
import { createForm } from 'rc-form';

const Item = List.Item;

let BasicInput = React.createClass({
  getInitialState() {
    return {
      value: 1,
    };
  },
  onSubmit() {
    this.props.form.validateFields({ force: true }, (error) => {
      if (!error) {
        console.log(this.props.form.getFieldsValue());
      } else {
        alert('校验失败');
      }
    });
  },
  onReset() {
    this.props.form.resetFields();
  },
  validateAccount(rule, value, callback) {
    if (value && value.length > 4) {
      callback();
    } else {
      callback(new Error('帐号至少4个字符'));
    }
  },
  render() {
    const { getFieldProps, getFieldError } = this.props.form;

    return (<form>
      <List renderHeader={() => '验证表单'}
        renderFooter={() => getFieldError('account') && getFieldError('account').join(',')}
      >
        <InputItem
          {...getFieldProps('account', {
            // initialValue: '小蚂蚁',
            rules: [
              { required: true, message: '请输入帐号' },
              { validator: this.validateAccount },
            ],
          })}
          clear
          error={!!getFieldError('account')}
          onErrorClick={() => {
            alert(getFieldError('account').join('、'));
          }}
          placeholder="请输入账号"
        >帐号</InputItem>
        <InputItem {...getFieldProps('password')} placeholder="请输入密码" type="password">
          密码
        </InputItem>
        <Item
          extra={<Switch {...getFieldProps('1', { initialValue: true, valuePropName: 'checked' })} />}
        >确认信息</Item>
        <Item><Slider range defaultValue={[20, 50]} /></Item>
        <Item extra={<Stepper style={{ width: '100%', minWidth: '2rem' }} showNumber size="small" defaultValue={20} />}>预定人数</Item>
      </List>
      <div style={{ margin: 12 }}>
        <Button type="primary" onClick={this.onSubmit} inline>提交验证</Button>
        <Button onClick={this.onReset} inline style={{ marginLeft: 5 }}>重置</Button>
      </div>
    </form>);
  },
});

BasicInput = createForm()(BasicInput);
ReactDOM.render(<BasicInput />, mountNode);
````
