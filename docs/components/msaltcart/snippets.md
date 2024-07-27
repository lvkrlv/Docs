# Сниппеты

 ## getCarts
Сниппет предназначен для вывода корзин на странице. Имеет только один параметр **tpls**, который принимает в качестве значения строку в формате JSON.
### Fenom
```fenom:line-numbers
{'!getCarts' | snippet: [
    'tpls' => '{
        "maincart":
        { 
            "wrapper":"@FILE msac/cart.tpl","row":"@FILE msac/cartrow.tpl" 
        } 
    }'
]}

{'maincart' | placeholder}
```
### Стандартный синтаксис
```modx:line-numbers
[[!getCarts?
    &tpls=`{ 
        "maincart": { 
            "wrapper":"@FILE msac/cart.tpl","row":"@FILE msac/cartrow.tpl" 
        } 
    }`
]]

[[+maincart]]
```

:::danger
Не смешивайте синтаксис: если сниппет вызван на страндартном синтаксисе, то плейсхолдеры вставляйте на стандартном. И не используйте в именах плейсхолдеров ничего кроме цифр и латиницы. 
:::

:::danger
Ключ объекта в параметре tpls и есть имя плейсхолдера для вставки в шаблоне.
:::

## msacConnector
Сниппет предназначен для обработки запросов с фронта. Его никогда и нигде не нужно вызывать.