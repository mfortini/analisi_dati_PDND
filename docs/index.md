---
toc: false
title: Analisi report PDND
theme: cotton
---

<div>

<h1>Analisi report PDND</h1>

Scegli il file .xlsx con il report di produzione:

```js
const xlsxfile = view(Inputs.file({label: "Excel file", accept: ".xlsx", required: true}));
```


```js
const workbook = xlsxfile.xlsx();
```


```js
const agreements = workbook.sheet('Agreements', {headers:true});
const descriptors = workbook.sheet('Descriptors', {headers:true});
const tokens = workbook.sheet('Tokens', {headers:true});
```

```js
const db = DuckDBClient.of({agreements: agreements, descriptors: descriptors, tokens:tokens});
```

```js
const count = db.queryRow("SELECT COUNT (DISTINCT AgreementId) AS numagreements FROM agreements");
```

${count["numagreements"]}


```js
//Inputs.table(descriptors)
```

```js
//Inputs.table(tokens)
```

```js
const aqagreements=aq.from(agreements);
const aqdescriptors=aq.from(descriptors);
const aqtokens=aq.from(tokens);
```



<h2>Metriche di base</h2>

* Ci sono:
  * ${aqagreements.numRows()} agreement di cui ${aqagreements.groupby('State').count().filter(d=>d.State == "Active").get("count")} attivi
  * ${aqdescriptors.numRows()} descriptor
  * ${aqtokens.numRows()} token

<h3>Agreements</h3>

${Inputs.table(aqagreements.groupby('State').count())}


<h2>Token</h2>


```js
const tot_token=db.query("SELECT date, SUM(CAST (tokencount AS INTEGER)) AS tot_token  FROM tokens WHERE date > '2024-01-01' GROUP BY date ORDER BY date");
```

${Plot.rectY(tot_token, {x:"date",y:"tot_token"}).plot()}


</div>

