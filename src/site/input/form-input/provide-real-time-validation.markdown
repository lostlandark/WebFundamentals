---
layout: article
title: "�ṩʵʱ��֤"
description: "ʵʱ������֤������֤���������Ŀ������ͬʱҲ��������û����顣Ŀǰ�����������ӵ��һЩ���ù����԰����ṩʵʱ��֤�����ҿ��Է�ֹ�û��ύ��Ч��Ϣ���Ӿ���ʾӦ�ñ�������ʾһ�����Ǳ���׼ȷ����д��"
introduction: "ʵʱ������֤������֤���������Ŀ������ͬʱҲ��������û����顣Ŀǰ�����������ӵ��һЩ���ù����԰����ṩʵʱ��֤�����ҿ��Է�ֹ�û��ύ��Ч��Ϣ���Ӿ���ʾӦ�ñ�������ʾһ�����Ƿ�׼ȷ����д��"
article:
  written_on: 2014-04-30
  updated_on: 2014-04-30
  order: 3
rel:
  gplusauthor: https://plus.google.com/+PeteLePage
collection: form-input
key-takeaways:
  provide-real-time-validation:
    - ������������õ�ʵʱ��֤���ԣ����磺
      <code>pattern</code>, <code>required</code>, <code>min</code>,
      <code>max</code>, �ȵȡ�
    - ʹ��JavaScript����������֤API��������ӵ���֤����
    - ʵʱ��ʾ��֤���󣬲�������û���ͼ�ύ��Ч������ô����ʾ���û���Ҫ�޸ĵ��ֶΡ�
remember:
  use-placeholders:
    - Placeholders disappear as soon as focus is placed in an element, thus
      they are not a replacement for labels.  They should be used as an aid
      to help guide users on the required format and content.
  recommend-input:
    - Auto-complete only works when the form method is post.
  use-datalist:
    - The <code>datalist</code> values are provided as suggestions, and users
      are not restricted to the suggestions provided.
  provide-real-time-validation:
    - ��ʹ�пͻ��˵�������֤����֤�������ϵ�������ȷ�������ݵ�ͳһ�ԺͰ�ȫ���Ծ��Ƿǳ���Ҫ�ġ�
  show-all-errors:
    - ��Ӧ�����û�һ����չʾ�����������⣬�����ǽ׶��Ե�չʾ��
  request-auto-complete-flow:
    - If you're asking for any kind of personal information or credit card
      data, ensure the page is served via SSL.  Otherwise the dialog will
      warn the user their information may not be secure.
---
{% wrap content %}

<style>
  table.inputtypes th:nth-of-type(2) {
    min-width: 270px;
  }

  table.tc-heavyright th:first-of-type {
    width: 30%;
  }
</style>

{% include modules/takeaway.liquid list=page.key-takeaways.provide-real-time-validation %}

### ʹ����Щ��������֤����

#### `pattern` ����

`pattern` ����ʹ��һ�� [������ʽ��regular
expression��](http://en.wikipedia.org/wiki/Regular_expression)����֤�����ֶ� �����磺��֤һ�������ʱࣨ5�����֣���ʱ������1�����ۺ��ټ���4����������֣������ǻ�����������`pattern` :

{% highlight html %}
<input type="text" pattern="^\d{5,6}(?:[-\s]\d{4})?$" ...>
{% endhighlight %}

##### ����������ʽpattern���±��Ϊ��������ֵ��

<table class="table-2 tc-heavyright">
  <thead>
    <tr>
      <th data-th="Description">����</th>
      <th data-th="Regular expression">������ʽ</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Description">ͨ�ŵ�ַ</td>
      <td data-th="Regular expression"><code>[a-zA-Z\d\s\-\,\#\.\+]+</code></td>
    </tr>
    <tr>
      <td data-th="Description">�ʱ�(����)</td>
      <td data-th="Regular expression"><code>^\d{5,6}(?:[-\s]\d{4})?$</code></td>
    </tr>
    <tr>
      <td data-th="Description">IP��ַ</td>
      <td data-th="Regular expression"><code>^(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$</code></td>
    </tr>
    <tr>
      <td data-th="Description">���ÿ�����</td>
      <td data-th="Regular expression"><code>^(?:4[0-9]{12}(?:[0-9]{3})?|5[1-5][0-9]{14}|3[47][0-9]{13}|3(?:0[0-5]|[68][0-9])[0-9]{11}|6(?:011|5[0-9]{2})[0-9]{12}|(?:2131|1800|35\d{3})\d{11})$</code></td>
    </tr>
    <tr>
      <td data-th="Description">��ᰲȫ���պ���</td>
      <td data-th="Regular expression"><code>^\d{3}-\d{2}-\d{4}$</code></td>
    </tr>
    <tr>
      <td data-th="Description">�����绰����</td>
      <td data-th="Regular expression"><code>^(?:(?:\+?1\s*(?:[.-]\s*)?)?(?:\(\s*([2-9]1[02-9]|[2-9][02-8]1|[2-9][02-8][02-9])\s*\)|([2-9]1[02-9]|[2-9][02-8]1|[2-9][02-8][02-9]))\s*(?:[.-]\s*)?)?([2-9]1[02-9]|[2-9][02-9]1|[2-9][02-9]{2})\s*(?:[.-]\s*)?([0-9]{4})(?:\s*(?:#|x\.?|ext\.?|extension)\s*(\d+))?$</code></td>
    </tr>
  </tbody>
</table>

#### `required` ����

���`required`���Գ��֣���ô���ֶ����ύǰ���������Ч��Ϣ�����磺Ҫ�������д�ʱ��ַ������ֻҪ����`required`����:

{% highlight html %}
<input type="text" required pattern="^\d{5,6}(?:[-\s]\d{4})?$" ...>
{% endhighlight %}

#### `min`, `max` and `step` ����

������ֵ�������ͣ������ֻ�Χ���������ڻ�ʱ�����룬�����������ŵ���ʱ������Զ������ǵ���Сֵ�����ֵ��ͬ���أ�Ҳ�ܹ��������ǲ���/���������磺Ь�ӵĳߴ�Ӧ��������СֵΪ1�����ֵΪ13������ֵ0.5��

{% highlight html %}
<input type="number" min="1" max="13" step="0.5" ...>
{% endhighlight %}

#### `maxlength` ����

`maxlength` �����ܹ�������˵��һ���������ı��������ַ����������������û��ṩ����Ϣ����ʱ���ǳ����á����磺�����������һ���ļ�����12���ַ����ڣ������ʹ��������ʾ��䣺
{% highlight html %}
<input type="text" id="83filename" maxlength="12" ...>
{% endhighlight %}

#### `novalidate` ����

��ĳЩ����£������Ҳ�������û��ύһ����������Ч����ı������Ը���Ԫ�ػ��ض������ֶ����novalidate���ԡ�����������£��������֤�����룬���е�α���JavaScript APIs��Ȼ���������顣

{% highlight html %}
<form role="form" novalidate>
  <label for="inpEmail">Email address</label>
  <input type="email" ...>
</form>
{% endhighlight %}

{% include modules/remember.liquid title="Remember" list=page.remember.provide-real-time-validation %}

### ʹ��JavaScript��Ӧ�Ը����ӵ�ʵʱ��֤

��������֤�����������ʽ����ʱ�������ʹ��[Constrains Validation API](http://dev.w3.org/html5/spec-preview/constraints.html#constraint-validation)������һ��ǿ�����Ĺ��ߣ����������Զ�����֤��API����������һ���Զ�����󣬼��Ԫ���Ƿ���Ч�������ܽ��һ��Ԫ����Ч��ԭ��

<table class="table-2 tc-heavyright">
  <thead>
    <tr>
      <th data-th="API">API</th>
      <th data-th="Description">����</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="API"><code>setCustomValidity()</code></td>
      <td data-th="Description">�����Զ�����֤��Ϣ��������<code>ValidityState</code>�����<code>customError</code>����Ϊ<code>true</code>��</td>
    </tr>
    <tr>
      <td data-th="API"><code>validationMessage</code></td>
      <td data-th="Description">����һ���ַ�����������Ϊ�����û��ͨ����֤��ԭ��</td>
    </tr>
    <tr>
      <td data-th="API"><code>checkValidity()</code></td>
      <td data-th="Description">���Ԫ�����������е����������򷵻�<code>true</code>������Ϊ<code>false</code>��</td>
    </tr>
    <tr>
      <td data-th="API"><code>validity</code></td>
      <td data-th="Description">����<code>ValidityState</code>��������ʾԪ�ص���Ч��״̬��</td>
    </tr>
  </tbody>
</table>

#### �����Զ�����֤��Ϣ

���һ���ֶ���֤ʧ�ܣ���ôʹ��`setCustomValidity()`�������Ч�ֶβ��ҽ���Ϊʲô���ֶ���Ч�����磺һ��ע������ܻ�Ҫ���û��������������ַ��ȷ�����ǵ��ʼ���ַ���ڵڶ�������ʹ��һ��ģ���¼�����֤������������ú��ʵ���ҳ��Ӧ�����磺

{% include_code _code/order.html customvalidation javascript %}

#### ��ֹ�ύ��Ч��

�������Ч���ݣ��������е�����������ֹ�û��ύ��������Ҫʹ�����ύ�¼������ڱ�Ԫ����ʹ��`checkValidity()`���жϱ��Ƿ���Ч������:

{% include_code _code/order.html preventsubmission javascript %}

### ��ʾʵʱ����

���û��ύ��ǰ��׼ȷ������˱���������ṩ�Ӿ���ʾ�Ǻ��а����ġ�HTML5ͬ����Ҳ�ṩ��һϵ�е�α�࣬������ֵ��������ȷ���������ʽ��

<table class="table-2 tc-heavyright">
  <thead>
    <tr>
      <th data-th="Pseudo-class">α��</th>
      <th data-th="Use">ʹ��</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Pseudo-class"><code>:valid</code></td>
      <td data-th="Use">������е�ֵ����֤Ҫ�������Ӧ�ø���ʽ��</td>
    </tr>
    <tr>
      <td data-th="Pseudo-class"><code>:invalid</code></td>
      <td data-th="Use">������е�ֵ����֤Ҫ�������Ӧ�ø���ʽ��</td>
    </tr>
    <tr>
      <td data-th="Pseudo-class"><code>:required</code></td>
      <td data-th="Use">�����Ԫ������required���Ե�Ӧ�ø���ʽ��</td>
    </tr>
    <tr>
      <td data-th="Pseudo-class"><code>:optional</code></td>
      <td data-th="Use">�����Ԫ��û����required���Ե�Ӧ�ø���ʽ��</td>
    </tr>
    <tr>
      <td data-th="Pseudo-class"><code>:in-range</code></td>
      <td data-th="Use">���������Ԫ�ص�ֵ�������跶Χ�е�Ӧ�ø���ʽ��</td>
    </tr>
    <tr>
      <td data-th="Pseudo-class"><code>:out-of-range</code></td>
      <td data-th="Use">���������Ԫ�ص�ֵ���������跶Χ�е�Ӧ�ø���ʽ��</td>
    </tr>
  </tbody>
</table>

����ҳ��ȡ��ϣ��ֶο����Ծ���Ч�������û���û�л��������ʱ����֤Ҳ��������ʼ����ͬʱҲ��ζ�ţ����û�����Ĺ����У�Ҳ��һֱ������Ч����ʽ��Ϊ�˷�ֹ�������������Խ�CSS��JavaScript��ϣ�ֻ���û����ʹ����ֶ�֮����ʾ��Ч��ʽ��

{% include_code _code/order.html invalidstyle css %}
{% include_code _code/order.html initinputs javascript %}

{% include modules/remember.liquid title="Important" list=page.remember.show-all-errors %}

{% include modules/nextarticle.liquid %}

{% endwrap %}
