---
title: 'Sõned ja kuupäevad'
description: 'Töö tekstiväärtustega ning kuupäevadega'
---

## Teksti esinemise kontroll

```yaml
type: NormalExercise
key: d68116f086
lang: r
xp: 100
skills: 1
```

Töölaual on andmestik `tekstid`, mis sisaldab tekstilõike ajalehe Postimees artiklitest. Kõigil lõikudel on ka emotsionaalne hinnang st kas lugejad on hinnanud lõigu negatiivseks, postiivseks, neutraalseks või vastuoluliseks.

Tekstilõigud on andmestikus tunnuse `tekst` nime all.

Andmed on pärit: http://peeter.eki.ee:5000/valence/paragraphsquery/


Töölaual on aktiveeritud pakett `stringr`.

`@instructions`
- **Ülesanne 1:** Kasutades paketi `stringr` sobivat käsku tuvasta, millistes tekstilõikudes esineb string 'Eesti' või 'eesti'. Tulemuseks peaks olema tõevektor ( väärtusega `TRUE` kui otsitav väärtus on tekstis ja väärtusega `FALSE` vstasel juhul), omista see muutujale `esineb`. Väike/suurtähe võimaluse otsitavas tekstis saab kirja panna järgnevalt `[Ss]uurtäht`.
- **Ülesanne 2:** Kasutades eelmises ülesandes tekitatud tunnust leia käsuga `table` sagedustabel, kus oleks näha tekstilõigu emotsionaalsete hinnangute kaupa ülalvaadatud stringi esinemissagedused. Määra emotsiooni tunnus sagedustabelis reatunnuseks. Tekkiv sagedustabel omista muutujale `sagedustabel`, prindi see ekraanile.
- **Ülesanne 3:** Kasutades eelnevalt tekitatud sagedustabeli-objekti leia tekstilõikude hinnangute jaotus mõlemas tekstilõikude grupis (st nii nende lõikude osas, kus stringi ei esinenud, kui selles grupis, kus esines). Omista saadud tabel muutujale `tinglikjaotus`, prindi see ekraanile.

`@hint`
- Esimeses ülesandes kasuta funktsiooni `str_detect` või  `str_count`, kus määra `pattern` argumendiks `[Ee]esti`. Käsk `str_count` annab tulemuseks esinemiste arvu, seega nõutud tõeväärtustega vektori saamiseks peab veel kontrollima, kas tulemused on üle nulli.
- Kolmandas ülesandes kasuta käsku `prop.table`, määrama peab ka selle, kas jaotus leida kogu tabeli summa või rea/veerusummade suhtes.

`@pre_exercise_code`
```{r}
# http://peeter.eki.ee:5000/valence/paragraphsquery/
# eluolu alajaotus, kõik emotsioonid,1000 esimest failist
tekstid  <- read.table("http://kodu.ut.ee/~annes/R/tekstid.csv",  sep =",", encoding = "UTF-8", nrows = 1000,
                col.names = c("rubriik", "artikkel", "loigunr", "hinnang", "tekst"))
tekstid$artikkel <- as.numeric(tekstid$artikkel )

library(stringr)

```

`@sample_code`
```{r}
# vaata andmestikku
str(tekstid)


# Ülesanne 1: tuvasta stringi esinemine
esineb <- ________(_____, _____)

# Ülesanne 2: sagedustabeli leidmine
sagedustabel <- table(_____, _____)
sagedustabel

# Ülesanne 3: osakaalude leidmine
tinglikjaotus <- ______._____(_____, _____)
tinglikjaotus

```

`@solution`
```{r}
# vaata andmestikku
str(tekstid)


# Ülesanne 1: tuvasta stringi esinemine
esineb <- str_detect(tekstid$tekst, pattern = "[Ee]esti")
# või ka
esineb.alt <- str_count(tekstid$tekst, pattern = "[Ee]esti") > 0 

# Ülesanne 2: sagedustabeli leidmine
sagedustabel <- table(tekstid$hinnang, esineb)
sagedustabel

# Ülesanne 3: osakaalude leidmine
tinglikjaotus <- prop.table(sagedustabel, 2)
tinglikjaotus
```

`@sct`
```{r}
#test_predefined_objects(tekstid, 
#                        eq_condition = "equivalent",
#                        eval = TRUE,
#                        undefined_msg = "Ära kustuta antud andmestikku!", 
#                        incorrect_msg = "Ära muuda antud andmestikku!")


#test_data_frame(tekstid,
#                columns = "tekst",
#                eq_condition = "equivalent",
#                undefined_msg = NULL,
#                undefined_cols_msg = "Kas kustutasid tekstilõigud andmestikust?",
#                incorrect_msg = "Oled tekstilõike muutnud. Alusta uuesti!")



#------------

# 1
test_or(
test_function(name = "str_detect",
              args = c("string", "pattern"),
              index = 1,
             eq_condition = "equivalent",
             not_called_msg = "Esimeses ülesandes saab kasutada funktsiooni `str_detect`.",
             args_not_specified_msg = paste("Käsus `str_detect` on vaja ", 
             c("esimeseks argumendiks panna tekstilõikude vektor", 
             "teiseks argumendiks panna otsitav string")),
             incorrect_msg = paste("Käsus `str_detect` on praegu ", 
             c("esimene argument  vale.",
             "teine argument   vale, määra otsitavaks stringiks `[E|e]esti`."))), 
test_function(name = "str_count",
              args = c("string", "pattern"),
              index = 1,
             eq_condition = "equivalent",
             not_called_msg = "Esimeses ülesandes saab kasutada funktsiooni `str_count`.",
             args_not_specified_msg = paste("Käsus `str_count` on vaja ", 
             c("esimeseks argumendiks panna tekstilõikude vektor", 
             "teiseks argumendiks panna otsitav string")),
             incorrect_msg = paste("Käsus `str_count` on praegu ", 
             c("esimene argument  vale.",
             "teine argument   vale, määra otsitavaks stringiks `[Ee]esti`.")))
)


test_object("esineb", 
            undefined_msg = "Muutuja  `esineb` on defineerimata.",
            incorrect_msg = "Muutuja  `esineb` väärtus ei ole korrektne. Proovi uuesti." )



# 2
test_function(name = "table", 
                args = NULL,
              index = 1,
             eq_condition = "equivalent",
             not_called_msg = "Teises ülesandes pead kasutama funktsiooni `table`.",
             args_not_specified_msg = NULL,
             incorrect_msg = "Funktsioonile `table` on antud valed argumendid.")


test_object("sagedustabel", 
            undefined_msg = "Tabeli  `sagedustabel` on defineerimata.",
            incorrect_msg = "Tabeli  `sagedustabel` väärtus ei ole korrektne. Proovi uuesti." )


test_output_contains("sagedustabel",
                     times = 1,
                     incorrect_msg = "Prindi sagredustabel ekraanile!")

# 3
test_function(name = "prop.table", 
            args = c("x", "margin"),
            index = 1,
             eq_condition = "equivalent",
             not_called_msg = "Kolmandas ülesandes pead kasutama funktsiooni `prop.table`.",
             args_not_specified_msg = paste("Käsu `prop.table` ", c("esimeseks argumendiks pane sageustabel", "teiseks argumendiks määra kas jaotus leitakse rea või veerusumma suhtes.") ),
             incorrect_msg = paste("Oled käsus `prop.table` ", c("argumendiks andmed vale sagedustabeli", " teise argumendi väärtuse valesti määranud, vaja on `margin = 2`.")))


test_object("tinglikjaotus", 
            undefined_msg = "Tabeli `tinglikjaotus` on defineerimata.",
            incorrect_msg = "Tabeli `tinglikjaotus` väärtus ei ole korrektne. Proovi uuesti." )


test_output_contains("tinglikjaotus",
                     times = 1,
                     incorrect_msg = "Tingik jaotus on ekraanile väljastamata.")



success_msg("Hästi!")




```

---

## Date-tüüpi muutuja loomine

```yaml
type: NormalExercise
key: 2e5c28c998
lang: r
xp: 100
skills: 1
```

Töölaual on andmestik `apelsinid`.

Mõõdetud on apelsinipuude tüve ümbermõõtu (`circumference`). Iga puud on mõõdetud korduvalt, mõõtmise aeg on veerus `age`, mis näitab puu vanust päevades mõõtmishetkel. Puu vanust hakati lugema alates katse algusest, mis oli 1968 aasta 31. detsember. Kõiki puid mõõdeti samadel päevadel.

`@instructions`
- **Ülesanne 1:** Lisa andmestikku uus tunnus nimega `kuupaev`, mis näitaks mõõtmiste kuupäevi kujul "YYYY-MM-DD".
- **Ülesanne 2:** Vali kuupäeva veerust unikaalsed väärtused ja omista muutujale `ajad`. Veendu, et need on kasvavas järjekorras.
- **Ülesanne 3:** Kasutades funktsiooni `difftime` ja eelmises sammus tehtud muutujat `ajad`, leia mõõtmistevahelised ajad nädalates. Tutvu ka funktsiooni `difftime` abifailiga. Arvutuste tulemus omista muutujale `nadalad`.

`@hint`
- Kuupäevad saad luua `as.Date` funtksiooniga, määrama peab ka `origin` argumendi - see on kuupäev milest alates päevi hakati loendama.
- Käsus `difftime` on kaks esimest argumenti ajavektorid: `ajad[-1]` ja `ajad[-length(ajad)]`. Kasutada on vaja argumenti `units`.

`@pre_exercise_code`
```{r}
library(nlme)
apelsinid <- as.data.frame(Orange)

```

`@sample_code`
```{r}
# Vaata andmestikku
head(apelsinid)


# Ülesanne 1: Lisa kuupäeva tunnus andmestikku
apelsinid$kuupaev <- ____._______(_____, origin = ________)


# Ülesanne 2: Moodusta unikaalsete väärtuste vektor
ajad <- ______(apelsinid$kuupaev)


# Ülesanne 3: Kui pikk on mõõtmistevaheline aeg nädalates?
nadalad <- difftime(_____, _____, units = _____)
nadalad

```

`@solution`
```{r}
# Vaata andmestikku
head(apelsinid)

# Ülesanne 1: Lisa kuupäeva tunnus andmestikku
apelsinid$kuupaev <- as.Date(apelsinid$age, origin = "1968-12-31")


# Ülesanne 2: Moodusta unikaalsete väärtuste vektor
ajad <- unique(apelsinid$kuupaev)


# Ülesanne 3: Kui pikk on mõõtmistevaheline aeg nädalates?
nadalad <- difftime(ajad[-1], ajad[-length(ajad)], units = "weeks")
nadalad

```

`@sct`
```{r}
#1
test_function(name = "as.Date", 
              args = c("x", "origin"),
              index = 1,
              eq_condition = "equivalent",
              not_called_msg = "Esimeses ülesandes saad kasutada funktsiooni `as.Date`.",
              args_not_specified_msg = c("Määra `as.Date` esimeseks argumendiks vektor `apelsinid$age`", "Pead `as.Date` funktsiooni lisama `origin` argumendi."),
              incorrect_msg =  c("Oled `as.Date` esimeseks argumendiks vale tunnuse andnud", 
                                 "Oled `as.Date` käsus vale `origin` argumendi väärtuse määranud. Kontrolli kuupäeva kirjapilti, see peab olema kujul *aasta-kuu-päev*."))


test_object("ajad", 
            undefined_msg = "Muutuja  `ajad` on defineerimata.",
            incorrect_msg = "Muutuja  `ajad` väärtus ei ole korrektne. Proovi uuesti. " )




test_data_frame("apelsinid", columns = "kuupaev",
            undefined_msg = "Andmetabel `apelsinid` on kustutatud!.",
            undefined_cols_msg = "Andmestikus `apelsinid` pole veergu nimega `kuupaev`.",
            incorrect_msg = "Andmetabeli `apelsinid`  veeru `kuupaev` väärtused ei ole korrektsed. Proovi uuesti." )





test_function(name = "difftime", 
              args = c("time1", "time2", "units"),
              index = 1,
              eq_condition = "equivalent",
              not_called_msg = "Viimases ülesandes kasuta  funktsiooni `difftime`.",
              args_not_specified_msg = c("Käsus `difftime` peab esimene argument olema ajavektor.",
										 "Käsus `difftime` peab teine argument ka olema ajavektor.",  
                                         "Määra `difftime` käsus argumendi `units` väärtus `'weeks'`."),
              incorrect_msg =  c("Käsus `difftime` on esimesel kohal olev ajavektor vale.", 
                                 "Käsus `difftime` on teisel kohal olev ajavektor vale.", 
                                 "Oled `difftime` käsus argumendi `units` väärtuse valesti määranud, ühikuks peab olema nädal."))
              

test_object("nadalad", 
            undefined_msg = "Muutuja `nadalad` on defineerimata.",
            incorrect_msg = "Muutuja `nadalad` väärtus ei ole korrektne. Proovi uuesti. " )


test_output_contains("nadalad",
                     times = 1,
                     incorrect_msg = "Muutuja `nadalad` väärtus on ekraanile printimata.")

#
success_msg("Hästi!")



```

---

## Kuupäevade võrdlemine

```yaml
type: NormalExercise
key: 30fdd7dec6
lang: r
xp: 100
skills: 1
```

Töölaual on andmestik `andmed`, mis sisaldab infot patsientide haiglasse saabumise(`haiglasse.kp`) ja lahkumise(`haiglast.kp`) kuupäevade kohta.

Ülesannetes peab moodustama 2 alamandmestikku.

`@instructions`
- **Ülesanne 1:** Kontrolli, kas andmestikus on vigaseid vaatlusi: ridu, kus patsient on haiglast lahkunud enne haiglasse saabumise kuupäeva. Vali selliste vaatluste read andmestikust ja omista muutujale `vead1`. Prindi tulemus ekraanile. 
- **Ülesanne 2:** Kontrolli, kas andmestikus on patsiente, kelle kohta pole teada nende haiglast lahkumise kuupäev. Vali selliste vaatluste read andmestikust ja omista muutujale `vead2`. Prindi tulemus ekraanile.

`@hint`
- Alamandestiku moodustamiseks võib kasutada käsku `subset` või konstruktsiooni `andmed[tingimus,]`. 
- Arvesta, et `NA` väärtus võrdluses (`==`) annab väärtuse `NA`. Meenuta, mida teeb funktsioon `is.na`.

`@pre_exercise_code`
```{r}
set.seed(126)
n <- 391
id <- sample(100:999, size = n)
kp  <- seq(as.Date("1997-01-01"), as.Date("2015-12-31"), 1)
haiglasse.kp <- sample(kp, size  = n)
haiglast.kp <- haiglasse.kp + rpois(n, lambda  = 5)
is.na(haiglast.kp) <- sample(rep(c(T, F), c(1, 100)), size = n, replace = T)
indeks <- sample(1:n, size = 5)
andmed <- data.frame(id, haiglasse.kp, haiglast.kp)
andmed[indeks, ] <- andmed[indeks, c(1, 3, 2)]

rm(n, id, kp, haiglasse.kp, haiglast.kp, indeks)




```

`@sample_code`
```{r}
# vaata andmestikku
str(andmed)


# Ülesanne 1: leia enne haiglasse tulekut lahkunud isikud
vead1 <- _________________________________
vead1



# Ülesanne 2: leia need, kelle haiglast lahkumise kuupäev pole teada
vead2 <- _________________________________
vead2


```

`@solution`
```{r}
# vaata andmestikku
str(andmed)


# Ülesanne 1: leia enne haiglasse tulekut lahkunud isikud
vead1 <- subset(andmed, andmed$haiglast.kp < andmed$haiglasse.kp)
vead1



# Ülesanne 2: leia need, kelle haiglast lahkumise kuupäev pole teada
vead2 <- subset(andmed, is.na(andmed$haiglast.kp))
vead2
```

`@sct`
```{r}
test_predefined_objects("andmed", 
                        eq_condition = "equivalent",
                        eval = TRUE,
                        undefined_msg = "Ära kustuta antud andmestikku!", 
                        incorrect_msg = "Ära muuda andmtud andmestikku!")

#-------------------

#1
test_object("vead1", 
            undefined_msg = "Andmestik  `vead1` on defineerimata.",
            incorrect_msg = "Andmestik  `vead1` väärtus ei ole korrektne. Proovi uuesti. Alamandmestiku valikuks kasuta näiteks käsku `subset`." )


test_output_contains("vead1",
                     times = 1,
                     incorrect_msg = "Andmestik `vead1`  on ekraanile printimata.")



#2
test_object("vead2", 
            undefined_msg = "Andmestik  `vead2` on defineerimata.",
            incorrect_msg = "Andmestik  `vead2` väärtus ei ole korrektne. Proovi uuesti. Puuduvate väärtuse kontrolliks saad kasutada käsku `is.na`." )


test_output_contains("vead2",
                     times = 1,
                     incorrect_msg = "Andmestik `vead2`  on ekraanile printimata.")



#
success_msg("Super! Viimane ülesanne sai tehtud. Kiida ennast!")


```
