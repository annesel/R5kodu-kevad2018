---
title       : Kuupäevad
description : Insert the chapter description here

--- type:NormalExercise lang:r xp:100 skills:1 key:2e5c28c998
## Date-tüüpi muutuja loomine

Töölaual on andmestik `apelsinid`.

Mõõdetud on apelsinipuude tüve ümbermõõtu (`circumference`). Iga puud on mõõdetud korduvalt, mõõtmise aeg on veerus `age`, mis näitab puu vanust päevades mõõtmishetkel. Puu vanust hakati lugema alates katse algusest, mis oli 1968 aasta 31. detsember. Kõiki puid mõõdeti samadel päevadel.


*** =instructions
- **Ülesanne 1** Lisa andmestikku uus tunnus nimega `kuupaev`, mis näitaks mõõtmiste kuupäevi kujul "YYYY-MM-DD".
- **Ülesanne 2** Vali kuupäeva veerust unikaalsed väärtused ja omista muutujale `ajad`.
- **Ülesanne 3** Kasutades funktsiooni `difftime` ja eelmises sammus tehtud muutujat `ajad`, leia mõõtmistevahelised ajad nädalates. Tutvu ka funktsiooni `difftime` abifailiga. Arvutuste tulemus omista muutujale `nadalad`.

*** =hint
- Kuupäevad saad luua `as.Date` funtksiooniga, määrama peab ka `origin` argumendi - see on kuupäev milest alates päevi hakati loendama.
- Käsus `difftime` on kaks esimest argumenti ajavektorid: `ajad[-1]` ja `ajad[-length(ajad)]`. Kasutada on vaja argumenti `units`.

*** =pre_exercise_code
```{r}
library(nlme)
apelsinid <- as.data.frame(Orange)

```

*** =sample_code
```{r}
# Vaata andmestikku
head(apelsinid)


# Ülesanne 1: Lisa kuupäeva tunnus andmestikku
apelsinid$kuupaev <- _______________________


# Ülesanne 2: Moodusta unikaalsete väärtuste vektor
ajad <- ______(apelsinid$kuupaev)


# Ülesanne 3: Kui pikk on mõõtmistevaheline aeg nädalates?
nadalad <- difftime(___________)
nadalad

```




*** =solution
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

*** =sct
```{r}
#1
test_function(name = "as.Date", 
              args = c("x", "origin"),
              index = 1,
              eq_condition = "equivalent",
              not_called_msg = "Esimeses ülesandes saad kasutada funktsiooni `as.Date`.",
              args_not_specified_msg = c("Määra `as.Date` esimeseks argumendiks vektor `age`", "Pead `as.Date` funktsiooni lisama `origin` arguemndi."),
              incorrect_msg =  c("Oled `as.Date` esimeseks argumendiks vvale tunnuse andnud", "Oled `as.Date`  käsus vale `origin` argumendi väärtuse määranud."))
              
test_data_frame("apelsinid", columns = "kuupaev",
            undefined_msg = "Andmetabel `apelsinid` on kustutatud!!.",
            undefined_cols_msg = "Andmestikus `apelsinid` pole veergu nimega `kuupaev`.",
            incorrect_msg = "Andmetabeli `apelsinid`  veeru `kuupaev` väärtused ei ole korrektsed. Proovi uuesti." )


test_object("ajad", 
            undefined_msg = "Muutuja  `ajad` on defineerimata.",
            incorrect_msg = "Muutuja  `ajad` väärtus ei ole korrektne. Proovi uuesti. " )





test_function(name = "difftime", 
              args = c("time1", "time2", "units"),
              index = 1,
              eq_condition = "equivalent",
              not_called_msg = "Viimases ülesandes kasuta  funktsiooni `difftime`.",
              args_not_specified_msg = c("Käsus `difftime` peab esimene argument olema ajavektor.","Käsus `difftime` peab teine argument ka olema ajavektor.",  
              "Määra `difftime` käsus argumendi `units` väärtus."),
              incorrect_msg =  c("Käsus `difftime` on esimesel kohal olev ajavektor vale.", "Käsus `difftime` on teisel kohal olev ajavektor vale.", "Oled `difftime` käsus argumendi `units` väärtuse valesti määranud."))
              

test_object("nadalad", 
            undefined_msg = "Muutuja `nadalad` on defineerimata.",
            incorrect_msg = "Muutuja `nadalad` väärtus ei ole korrektne. Proovi uuesti. " )


test_output_contains("nadalad",
                     times = 1,
                     incorrect_msg = "Muutuja `nadalad` väärtus on ekraanile printimata.")

#
success_msg("Hästi!")



```



<!--- vahe--->

--- type:NormalExercise lang:r xp:100 skills:1 key:30fdd7dec6
## Kuupäevade võrdlemine

Töölaual on andmestik `andmed`, mis sisaldab infot patsientide haiglasse saabumise(`haiglasse.kp`) ja lahkumise(`haiglast.kp`) kuupäevade kohta.

Ülesannetes peab moodustama 2 alamandmestikku.


*** =instructions
- **Ülesanne 1** Kontrolli, kas andmestikus on vigaseid vaatlusi: ridu, kus patsient on haiglast lahkunud enne haiglasse saabumise kuupäeva. Vali selliste vaatluste read andmestikust ja omista muutujale `vead1`. Prindi tulemus ekraanile. 
- **Ülesanne 2** Kontrolli, kas andmestikus on patsiente, kelle kohta pole teada nende haiglast lahkumise kuupäev. Vali selliste vaatluste read andmestikust ja omista muutujale `vead2`. Prindi tulemus ekraanile.

*** =hint
- Alamandestiku moodustamiseks võib kasutada käsku `subset` või konstruktsiooni `andmed[tingimus,]`. 
- Arvesta, et `NA` väärtus võrdluses annab väärtuse `NA`.

*** =pre_exercise_code
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

*** =sample_code
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

*** =solution
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





*** =sct
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







