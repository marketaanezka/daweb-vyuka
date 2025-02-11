Ve vanilla JavaScriptu jsem se nejdříve učili vytvářet statický obsah stránky a později jsme postupně přidávali interaktivitu. V Reactu budeme postupovat stejně. V této lekci uděláme další krok k tomu, aby naše stránky mohly být dynamičtější. Bude se nám k tomu hodit jedna hezká pomůcka z čistého JavaScriptu, kterou jsme v předešlých lekcích vynechali.

## Ternární operátor

Představte si situaci, kdy chceme uživateli zobrazit jednoduchou zprávu podle jeho věku.

```js
let message = null;
if (age >= 18) {
  message = 'Smíš vstoupit';
} else {
  message = 'Utíkej za mamkou';
}
```

Tato podmínka vypadá velmi přímočaře. Má však určité nevýhody.

1. Jde o celkem dlouhý kód pro velmi jednoduchou věc.
1. Musíme požívat proměnnou `let`, čemuž se snažíme co nejvíce vyhýbat.

JavaScript nám pro tuto situaci nabízí zkraktu, které se říká :term{cs="operátor pro podmíněný výraz" en="conditional operator"}.

```js
const message = age >= 18 ? 'Smíš vstoupit' : 'Utíkej za mamkou';
```

Tento operátor se dá použít vždy, když chceme do nějaké proměnné uložit různé hodnoty na základně nějaké podmínky.

V Reactu se nám tento operátor bude hodit ve více situacích. První z nich je situace, kdy chceme zkonstruovat název CSS třídy podle nějaké podmínky. Vezměme položku nákupního seznamu napsanou jako React komponentu.

```js
const ShoppingItem = (props) => {
  return (
    <div className="item">
      <span className="item__name">{props.name}</span>
      <span className="item__amount">{props.amount}</span>
    </div>
  );
};
```

Dejme tomu, že chceme být schopni položku vysvítit jako vybranou. Můžeme tedy komponentě přidat novou `prop` s názvem `selected` a použít ji takto.

```js
<ShoppingList name="jablka" amount="1 kg" selected={true} />
```

Budeme pak mít CSS třídu `item--selected`, která například nastavuje jinou barvu pozadí. Vybraná položka by tak měla mít atribut `className` nastaven takto.

```js
<div className="item item--selected">
```

Obsah atributu `className` tedy chceme zkonstruovat dle hodnoty `props.selected`. To bychom mohli udělat pomocí podmínky.

```js
const ShoppingItem = (props) => {
  let itemClass = null;
  if (props.selected) {
    itemClass = 'item item--selected';
  } else {
    itemClass = 'item';
  }

  return (
    <div className={itemClass}>
      <span className="item__name">{props.name}</span>
      <span className="item__amount">{props.amount}</span>
    </div>
  );
};
```

Díky podmíněnému operátoru si situaci můžeme zjednodušit takto.

```js
const ShoppingItem = (props) => {
  const itemClass = props.selected ? 'item item--selected' : 'item';

  return (
    <div className={itemClass}>
      <span className="item__name">{props.name}</span>
      <span className="item__amount">{props.amount}</span>
    </div>
  );
};
```

Dokonce bychom hodnotu ani nemuseli ukládat do proměnné a použít podmíněný operátor přímo na místě.

```js
const ShoppingItem = (props) => {
  return (
    <div className={props.selected ? 'item item--selected' : 'item'}>
      <span className="item__name">{props.name}</span>
      <span className="item__amount">{props.amount}</span>
    </div>
  );
};
```

Takovýto kód už však může být hůře čitelný, takže je dobré jej používat s mírou a uvážením.
