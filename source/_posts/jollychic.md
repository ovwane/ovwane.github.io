---
date: 2018-05-08 20:32:33
---

新建项目和crawlspider爬虫

```shell

scrapy startproject shopping
cd shopping
scrapy genspider -t crawl jollychic www.jollychic.com
```

设置settings.py 项目的路径

```python
import sys
import os
project_dir = os.path.abspath(os.path.dirname(__file__))
BASE_DIR = os.path.dirname(os.path.abspath(os.path.dirname(__file__)))
sys.path.insert(0, os.path.join(BASE_DIR, 'shopping'))
```

items.py

```python
from scrapy import Item, Field
from scrapy.loader import ItemLoader


class JollychicItemLoader(ItemLoader):
    default_output_processor = TakeFirst()
class JollychicItem(Item):
    title = Field()
```



spiders/jollychic.py

```python
from item import JollychicItemLoader, JollychicItem

def parse(self, response):
    item_loader = JollychicItemLoader(item=JollychicItem, response=reponse)
    item_loader.add_css()
    item = item_loader.load_item()
    return item
```

