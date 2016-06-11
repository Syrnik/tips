# tips

## Информация о купоне по его ID

Хелпер, помогающий извлечь информацию о купоне, если известен его идентификатор. Например, в заказе хранится только
идентификатор использованного купона и нет возможности получить код и прочие данные о купоне.

Вызов хелпера:

    {$coupon_info = shopTipsPlugin::getCouponById(4)}
    
в примере цифра 4 это ID купона. Например в печатных формах можно получить ID использованного в заказе купона из
переменной `$order.params.coupon_id`. Обратите внимание, что во-первых в заказе может вовсе не быть купона, а
во-вторых купон может быть удален после оформления заказа.

**Возвращаемое значение**. Если купон найден, то возвращается массив формата ключ-значение:
    
* *code* - код купона
* *type* - тип. может быть строкой '%', 3-х символьным кодом валюты или '$FS'. Символ процента означает
  скидку в процентах, код валюты - скидку на сумму в указанной валюте, значение '$FS' - бесплатную доставку
* *limit* - ограничение на количество использований. Если ограничений нет, то `NULL`
* *used* - сколько раз использован купон. Таким образом, если *limit* не равно `NULL` и *used* равно или больше *limit*
  то купон неактивен
* *comment* - строка с комментарием к купону
* *extire_datetime* - ограничение по дате использования. Если ограничения по дате нет, то `NULL`

Пример кода для использования в печатной форме:

    {if !empty($order.params.coupon_id)}
        {$coupon_info = shopTipsPlugin::getCouponById($order.params.coupon_id)}
        {if !empty($coupon_info}
        <p>Использован купон <b>{$coupon_info['code']}</b></p>
        {/if}
    {/if}
