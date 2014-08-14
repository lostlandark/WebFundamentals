---
layout: article
title: "ѡ�������������"
description: "ÿһ�ε�����������ڣ��û����л��Щ��վ������������绰�����ʱ���Զ����ֲ��ż��̣�������������������Ϣǰ��ǰ��䡣����ı����Ѱ���ܹ�ȥ����������Ļ��ᡣ"
introduction: "ÿһ�ε�����������ڡ��û����л��Щ��վ������������绰�����ʱ���Զ����ֲ��ż��̣�������������������Ϣǰ��ǰ�����ı����Ѱ���ܹ�ȥ����������Ļ��ᡣ"
article:
  written_on: 2014-04-30
  updated_on: 2014-04-30
  order: 1
rel:
  gplusauthor: https://plus.google.com/+PeteLePage
collection: form-input
key-takeaways:
  choose-best-input-type:
    - Ϊ�������ѡ�����ʺϵ����������Լ��������̡�
    - ʹ��<code>datalist</code> Ԫ��Ϊ�û����������ṩ����.
remember:
  use-placeholders:
    - �����㼯����һ��Ԫ����ʱ��ռλ���ͻ���ʧ��������ǲ��������ǩ��
      ռλ��Ӧ����Ϊһ�ְ��������û�����ĸ�ʽ�������Ͻ��е�����
  recommend-input:
    - �Զ����ֻ���ڵ������ֵ���������С�
  use-datalist:
    - <code>datalist</code> ֵ��Ϊ�����ṩ���û��������û�������������ṩ�Ľ����С�
  provide-real-time-validation:
    - ��ʹ���ڿͻ���һ����������֤���ڷ���������֤������ȷ��������ݵ�ͳһ�ԺͰ�ȫ��ͬ����Ҫ��
  show-all-errors:
    - ��Ӧ��һ���Ը����û������ڵ��������⣬�����Ƿ�ʱ�θ����û���
  request-auto-complete-flow:
    - �����������Ѱ�κ�˽����Ϣ�����ÿ����ݣ���ô��ȷ�����ҳ����SSL�����С�
      ����Ի���ᾯ���û����ǵ���Ϣ���ܲ��ܵõ���ȫ���ϡ�
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

{% include modules/takeaway.liquid list=page.key-takeaways.choose-best-input-type %}

### HTML5 ��������

HTML5 ������һϵ���µ��������͡���Щ�·�����ʾ�����Ӧ��Ϊ��Ļ������ʾ�����ļ��̲��֡��û��ܹ�������������������Ϣ���������л����ǵļ��̣����ң��û�ֻ�ῴ���ʺϵ�ǰ�������͵İ�����

<table class="table-2 inputtypes">
  <thead>
    <tr>
      <th data-th="Input type">����<code>����</code></th>
      <th data-th="Typical keyboard">��׼����</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Input type">
        <code>url</code><br>����һ����ַ����������Ч����ַ��Ͽ�ʼ�����磺 <code>http://</code>, <code>ftp://</code>����<code>mailto:</code>.
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/url-ios.png" srcset="imgs/url-ios.png 1x, imgs/url-ios-2x.png 2x">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>tel</code><br>����绰���룬<b>����</b>����ǿ���ض�����Ч�ṹ�������������ȷ��һ���ض���ʽ�������ʹ��pattern��
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/tel-android.png" srcset="imgs/tel-android.png 1x, imgs/tel-android-2x.png 2x">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>email</code><br>�����ʼ���ַ��@����Ӧ��Ĭ�ϳ����ڼ����ϡ����Ҫ�ṩ��ֹһ���ʼ���ַ���������Ӷ�����ʵ�֡�
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/email-android.png" srcset="imgs/email-android.png 1x, imgs/email-android-2x.png 2x">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>search</code><br>�ı���������ķ���ƽ̨��������ķ����һ�µġ�
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/plain-ios.png" srcset="imgs/plain-ios.png 1x, imgs/plain-ios-2x.png 2x" class="keybimg">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>number</code><br>������ֵ����Щ���ֿ������κκ�����������Ǹ�������
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/number-android.png" srcset="imgs/number-android.png 1x, imgs/number-android-2x.png 2x" class="keybimg">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>range</code><br>������ֵ��������ֵ������Ҫ����ͬ�������������͡������Ի���ؼ�����ʽ��ʾ���û���ǰ��
</td>
      <td data-th="Typical keyboard">
        <img src="imgs/range-ios.png">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>datetime-local</code><br>�ڱ���ʱ��������һ������ֵ��ʱ��ֵ��
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/datetime-local-ios.png" srcset="imgs/datetime-local-ios.png 1x, imgs/datetime-local-ios-2x.png 2x">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>date</code><br>������һ������ֵ�����Ҳ��ṩʱ����
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/date-android.png" srcset="imgs/date-android.png 1x, imgs/date-android-2x.png 2x">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>time</code><br>������һ��ʱ��ֵ�����Ҳ��ṩʱ����
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/time-ios.png" srcset="imgs/time-ios.png 1x, imgs/time-ios-2x.png 2x">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>week</code><br>������һ������ֵ�����Ҳ��ṩʱ����      
</td>
      <td data-th="Typical keyboard">
        <img src="imgs/week-android.png" srcset="imgs/week-android.png 1x, imgs/week-android-2x.png 2x">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>month</code><br>������һ���·�ֵ�����Ҳ��ṩʱ����
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/month-ios.png" srcset="imgs/month-ios.png 1x, imgs/month-ios-2x.png 2x">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>color</code><br>ѡ��һ����ɫ��
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/color-android.png" srcset="imgs/color-android.png 1x, imgs/color-android-2x.png 2x">
      </td>
    </tr>
  </tbody>
</table>

### ������ʱ��datalist�ṩ����

`datalist`Ԫ�ز�����һ���������ͣ�����һ�������������ݵ��б����ͱ��ֶ���ء����û������ʱ��������������û��Ƽ��Զ���ɵ�ѡ�����ѡ��Ԫ�أ��û���������߳����б�����Ѱ������Ѱ�ҵ�ֵ�����������û�ͣ�����б��ϡ�`datalist`ֻ���û�����ʱ���ṩ��ʾ��

{% include_code _code/order.html datalist %}

{% include modules/remember.liquid title="Remember" list=page.remember.use-datalist %}

{% include modules/nextarticle.liquid %}

{% endwrap %}
