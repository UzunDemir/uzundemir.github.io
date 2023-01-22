---
layout: post
title:  "Как бороться с SettingWithCopyWarning в Pandas?"
---
Выполняя работу по оценке модели и подготовке к передаче заказчику в модуле появилась предупреждение.

Как все выглядело:

![Image alt]({{ site.baseurl }}//images/screenshot 22_01_23_1)


Что это означает и как от этого избавится?

Был создан SettingWithCopyWarning, чтобы помечать потенциально запутанные «цепочечные» назначения, такие как следующие, 
которые не всегда работают должным образом, особенно когда первый выбор возвращает копию.

*[ИЗдесь дается объяснение происходящего](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy)*


Устраняется это вот таким методом, который описан [здесь.](https://stackoverflow.com/questions/20625582/how-to-deal-with-settingwithcopywarning-in-pandas)
