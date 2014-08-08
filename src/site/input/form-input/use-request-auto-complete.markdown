---
layout: article
title: "ʹ�� requestAutocomplete API�򻯽�������"
description: ����`requestAutocomplete`����Ƴ�����Ϊ�˰����û���д����������ڵ�����������Ϊ�㷺ʹ�á����������е�������<a href='http://seewhy.com/97-shopping-cart-abandonment-rate-mobile-devices-concern-you/'>�ߴ�97%</a>��
introduction: "`requestAutocomplete`����Ƴ�����Ϊ�˰����û���д����������ڵ�����������Ϊ�㷺ʹ�á����������е�������<a href='http://seewhy.com/97-shopping-cart-abandonment-rate-mobile-devices-concern-you/'>�ߴ�97%</a>�� ����һ�£���һ�������97%���˵ù��ﳵ��װ����������Ҫ�ĸ��ֶ����������������һ�ţ�Ȼ���뿪�˳��С�"
article:
  written_on: 2014-04-30
  updated_on: 2014-04-30
  order: 4
rel:
  gplusauthor: https://plus.google.com/+PeteLePage
collection: form-input
key-takeaways:
  use-request-auto-complete:
    - <code>requestAutocomplete</code> �ܹ����̶��ϼ򻯽��˵Ĺ��̣�ͬʱ�ܹ������û����顣
    - �����ҳ��<code>requestAutocomplete</code>���ã��������ؽ��˱���ֱ�������û�����ȷ��ҳ�档
    - ȷ�������ǩ�������ʵ��Զ�������ԡ�
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
    - Even with client-side input validation, it is always important to
      validate data on the server to ensure consistency and security in your data.
  show-all-errors:
    - You should show the user all of the issues on the form at once, rather than showing them one at a time.
  request-auto-complete-flow:
    - ����������κ������˽����Ϣ�������ÿ����ݣ���ȷ����ҳ����SSL����״̬�����򣬽��ᵯ���Ի��򾯸��û����ǵ���Ϣ���ܻ᲻��ȫ��
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

{% include modules/takeaway.liquid list=page.key-takeaways.use-request-auto-complete %}

������Ҫ�����ض�֧���ֶε���վ��`requestAutocomplete`��Ҫ����������֧��ϸĿ�������������ջ���ַ�Լ����ÿ���Ϣ������ЩϸĿѡ���Եر���������棬����������Զ���ɱ��ǳ�����

### `requestAutocomplete`����

��Ϊ��������鷽ʽ����վ����ʾ`requestAutocomplete`�ĶԻ�������Ƕ�ȡչʾ���ʱ���ҳ�档���һ��˳�����û������ῴ����������Ժ����׵�Ϊ�Ѵ��ڵı����`requestAutoComplete`���ⲻ��Ҫ�ı��κ��ֶ����֡��򵥵ظ�ÿһ����Ԫ�����`autocomplete`���ԣ�����Ӻ��ʵ�ֵ���Լ��ڱ�Ԫ�������`requestAutocomplete()`��������������Զ����ʣ�µ��¡�
<img src="imgs/rac_flow.png" class="center" alt="Request autocomplete flow">

{% include_code _code/rac.html rac javascript %}

`form`Ԫ���е�`requestAutocomplete`�������������Ӧ����������Ϊһ����ȫ���ԣ������������ͨ���û����ƣ����紥��������굥���������ѡ�Ȼ�󣬵���һ���Ի���ѯ���û����������ֶ��Լ���Щ�û���Ҫ������ϸ���ݡ�


{% include_code _code/rac.html handlerac javascript %}

��`requestAutocomplete`��ɵ�ʱ��������������ڱ��ɹ����ɹ�ʱ����`autocomplete`�¼��������ڱ����ܹ���ɵ�����¼���`autocompleteerror`�¼���������ɹ���䲢��ȷ��������Ҫ�ģ��Ǿ��ύ��Ȼ���������ȷ�ϡ�

{% include modules/remember.liquid title="Remember" list=page.remember.request-auto-complete-flow %}

{% include modules/nextarticle.liquid %}

{% endwrap %}
