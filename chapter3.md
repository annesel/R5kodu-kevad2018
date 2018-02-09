---
title       : Sõnetöötlus
description : Töö tekstiväärtustega

--- type:NormalExercise lang:r xp:100 skills:1 key:d68116f086
## Teksti esinemine

Töölaual on andmestik `tekstid`, mis sisaldab tekstilõike ajalehe Postimees artiklitest. Kõigil lõikudel on ka emotsionaalne hinnang st kas lugejad on hinnanud lõigu negatiivseks, postiivseks, neutraalseks või vastuoluliseks.

Tekstilõigud on andmestikus tunnuse `tekst` nime all.

Andmed on pärit: http://peeter.eki.ee:5000/valence/paragraphsquery/


Pakett `stringr` on juba aktiveeritud.




*** =instructions
- **Ülesanne 1** Kasutades paketi `stringr` sobivat käsku tuvasta, millistes tekstilõikudes esineb string 'Eesti' või 'eesti'. Tulemuseks peaks olema tõevektor (`TRUE` kui otsitav väärtus on tekstis), omista see muutujale `esineb`. Väike/suurtähe võimaluse otsitavas tekstis saab kirja panna järgnevalt `[Ss]uurtäht`.
- **Ülesanne 2** Kasutades eelmises ülesandes tekitatud tunnust leia käsuga `table` sagedustabel, kus oleks näha tekstilõigu emotsionaalsete hinnangute kaupa ülalvaadatud stringi esinemissagedused.
Määra emotsiooni tunnus sagedustabelis reatunnuseks. Tekkiv sagedustabel omista muutujale `sagedustabel`, prindi see ekraanile.
- **Ülesanne 3** Kasutades eelnevalt tekitatud sagedustabeli-objekti leia tekstilõikude hinnangute jaotus mõlemas tekstilõikude grupis (st nii nende lõikude osas, kus stringi ei esinenud, kui selles grupis, kus esines). Omista saadud tabel muutujale `tinglikjaotus`, prindi see ekraanile.

*** =hint
- Esimeses ülesandes kasuta funktsiooni `str_detect` või  `str_count`, kus määra `pattern` argumendiks `[Ee]esti`. Käsk `str_count` annab tulemuseks esinemiste arvu, seega nõutud tõeväärtustega vektori saamiseks peab veel kontrollima, kas tulemused on üle nulli.
- Kolmandas ülesandes kasuta käsku `prop.table`, määrama peab ka selle, kas jaotus leida kogu tabeli summa või rea/veerusummade suhtes.

*** =pre_exercise_code
```{r}
# http://peeter.eki.ee:5000/valence/paragraphsquery/
# eluolu alajaotus, kõik emotsioonid,1000 esimest failist
tekstid  <- read.table("http://kodu.ut.ee/~annes/R/tekstid.csv",  sep =",", encoding = "UTF-8", nrows = 1000,
                col.names = c("rubriik", "artikkel", "loigunr", "hinnang", "tekst"))
tekstid$artikkel <- as.numeric(tekstid$artikkel )

library(stringr)

```

*** =sample_code
```{r}
# vaata andmestikku
str(tekstid)


# Ülesanne 1: tuvasta stringi esinemine
esineb <- _______________________

# Ülesanne 2: sagedustabeli leidmine
sagedustabel <- table(______________)
sagedustabel

# Ülesanne 3: osakaalude leidmine
tinglikjaotus <- __________________
tinglikjaotus

```



*** =solution
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

*** =sct
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























