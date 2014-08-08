---
layout: article
title: "���ʵı�ǩ�����������"
description: "���������ƶ��豸����ȫ���ֳ�����������ѵı�����Щ������������ı���"
introduction: "���������ƶ��豸����ȫ���ֳ�����������ѵı�����Щ������������ı����õı��ܹ��ṩ���û�ǡ��ʱ�˵��������͡�����Ӧ�øı�������ƥ���û������볡�������û������������һ���¼�ʱ����ʹ����û�ʱ�̱��ֱ����ѵ�״̬����֤������Ӧ�ø����û����ύ���ǰ��Ҫ��Щʲô��"
article:
  written_on: 2014-04-30
  updated_on: 2014-04-30
  order: 2
rel:
  gplusauthor: https://plus.google.com/+PeteLePage
collection: form-input
key-takeaways:
  label-and-name:
    - �ڱ��������ʹ��<code>label</code>��ͬʱ��֤������������ڼ���ʱ�ǿɼ��ġ�
    - ʹ�� <code>placeholder</code>Ϊ�û��ṩ�����ڴ���ָ����
    - ����������Զ���ɱ���ʹ���ѽ�����<code>name</code>
      ��ΪԪ�ز�ʹ��<code>autocomplete</code> ���ԡ�
remember:
  use-placeholders:
    - ��Ԫ���е����ݱ��滻ʱ��ռλ�����Զ����ء���ˣ�ռλ�����������ǩ������Ӧ�ñ�������Ϊ�����ʽ�������ϵ�һ�ְ�����ָ���û���
  recommend-input:
    - ֻ�е�������Ϊ�ύ��ʱ���Զ���ɲŻṤ����
  use-datalist:
    - The <code>datalist</code> values are provided as suggestions, and users
      are not restricted to the suggestions provided.
  provide-real-time-validation:
    - Even with client-side input validation, it is always important to
      validate data on the server to ensure consistency and security in your data.
  show-all-errors:
    - You should show the user all of the issues on the form at once, rather than showing them one at a time.
  request-auto-complete-flow:
    - If you're asking for any kind of personal information or credit card
      data, ensure the page is served via SSL.  Otherwise the dialog will
      warn the user their information may not be secure.
---
{% wrap content %}

<style>
  img, video, object {
    max-width: 100%;
  }

  img.center {
    display: block;
    margin-left: auto;
    margin-right: auto;
  }

  table.inputtypes th:nth-of-type(2) {
    min-width: 270px;
  }

  table.tc-heavyright th:first-of-type {
    width: 30%;
  }
</style>

{% include modules/takeaway.liquid list=page.key-takeaways.label-and-name %}

### ��ǩ����Ҫ��

`label` Ԫ��Ϊ�û��ṩ���򣬸���������һ����Ԫ���е�������Ϣ��ʲô��ͨ����`label`Ԫ���з�������򣬻���ʹ�á�`for`�����ԣ�ÿһ��`label`�;ͻ��ÿһ��������໥������ͨ������Ԫ����ӱ�עͬ��Ҳ���԰���������Ŀ��������������û��ѽ��㼯�����������ʱ�����Դ�����ע�����������ķ�ʽ��ʵ�֡� 
{% include_code _code/order.html labels %}

### ��ǩ�ߴ���λ��

��ǩ�������Ӧ�����㹻��ĳߴ��Ա������������ӿ��У���ǩ�ֶ�Ӧ��λ���������Ϸ����ں����ӿ��ڣ���ǩ�ַ�Ӧ��λ��������һ�ԡ���ȷ����ǩ�ֶ�������Ӧ�������ͬʱ�ɼ�����ע���Զ������������������ܽ�������ƶ���ҳ�涥�������ر�ǩ��������λ��������·��ı�ǩ�ᱻ������̶��ڸǡ�

### ʹ��ռλ��

ռλ������Ϊ�û��ṩ����������ʾ����ͨ�������ı���ʾֱ��Ԫ�ر�ѡ��Ϊֹ��

<input type="text" placeholder="MM-YYYY">

{% highlight html%}
<input type="text" placeholder="MM-YYYY" ...>
{% endhighlight %}


{% include modules/remember.liquid title="Remember" list=page.remember.use-placeholders %}

### ʹ��Ԫ����ʹ�Զ���ɳ�Ϊ����

����վΪ�û���ʡʱ�䣬�Զ�����ͨ���ַ������磺���֡��ʼ���ַ���������õı��ַ�ʱ���û���е��ǳ��м������Ԫ�����ܹ���������Ǳ�ڵ�������� - �ر�����������̺�С���豸�ϡ�

�������[�����û�֮ǰ��������ض�����](https://support.google.com/chrome/answer/142893)��ʹ�ö�������ʽ������������Щ�ֶο���[�Զ����](https://support.google.com/chrome/answer/142893)�����ң�ͨ������������ṩ�������Ժ��Զ�������ԣ�����Ը����������ʾ�� 

������˵��Ϊ����ʾ�������Ӧ���Զ�����û��������ʼ���ַ���ֻ����룬��Ӧ��ʹ��:

{% include_code _code/order.html autocomplete %}


### �Ƽ��������`name`��`autocomplete`����ֵ

<table class="table-3 autocompletes">
  <thead>
    <tr>
      <th data-th="Content type">��������</th>
      <th data-th="name attribute"><code>name</code>����</th>
      <th data-th="autocomplete attribute"><code>autocomplete</code>����</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Content type">����</td>
      <td data-th="name attribute">
        <code>name</code>
        <code>fname</code>
        <code>mname</code>
        <code>lname</code>
      </td>
      <td data-th="autocomplete attribute"><code>name</code></td>
    </tr>
    <tr>
      <td data-th="Content type">Email</td>
      <td data-th="name attribute"><code>email</code></td>
      <td data-th="autocomplete attribute"><code>email</code></td>
    </tr>
    <tr>
      <td data-th="Content type">��ַ</td>
      <td data-th="name attribute">
        <code>address</code>
        <code>city</code>
        <code>region</code>
        <code>province</code>
        <code>state</code>
        <code>zip</code>
        <code>zip2</code>
        <code>postal</code>
        <code>country</code>
      </td>
      <td data-th="autocomplete attribute">
        <code>street-address</code>
        <code>locality</code>
        <code>region</code>
        <code>postal-code</code>
        <code>country</code>
      </td>
    </tr>
    <tr>
      <td data-th="Content type">�绰</td>
      <td data-th="name attribute">
        <code>phone</code>
        <code>mobile</code>
        <code>country-code</code>
        <code>area-code</code>
        <code>exchange</code>
        <code>suffix</code>
        <code>ext</code>
      </td>
      <td data-th="autocomplete attribute"><code>tel</code></td>
    </tr>
    <tr>
      <td data-th="Content type">���ÿ�</td>
      <td data-th="name attribute">
        <code>ccname</code>
        <code>cardnumber</code>
        <code>cvc</code>
        <code>ccmonth</code>
        <code>ccyear</code>
        <code>exp-date</code>
        <code>card-type</code>
      </td>
      <td data-th="autocomplete attribute">
        <code>cc-name</code>
        <code>cc-number</code>
        <code>cc-csc</code>
        <code>cc-exp-month</code>
        <code>cc-exp-year</code>
        <code>cc-exp</code>
        <code>cc-type</code>
      </td>
    </tr>
  </tbody>
</table>

`autocomplete`����Ӧ�ô���`shipping`��`billing`ǰ׺�����������ݾ�����

{% include modules/remember.liquid title="Remember" list=page.remember.recommend-input %}

### `autofocus`����

��һЩ���У�������Google��ҳ����Ψһ�����û�������������д�ض��ֶΣ������������`autofocus`���ԡ�����Ӹ����Ժ�������������������������������ֶΣ�ʹ���û��������ɿ��ٿ�ʼʹ�ñ����ƶ����������`autofocus`���ԣ�����Ϊ�˷�ֹ����������ֵ������

��ʹ���Զ���ȡ��������ʱ��ע�⣬��Ϊ������ռ���̽��㣬��Ǳ�ڵؽ����������������˸����

{% highlight html %}
<input type="text" autofocus ...>
{% endhighlight %}

{% include modules/nextarticle.liquid %}

{% endwrap %}
