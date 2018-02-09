---
title       : Pakett data.table
description : Andmetöötluspaketi data.table kasutamine

--- type:NormalExercise lang:r xp:100 skills:1 key:d4e4f82648
## Tunnuse tüübi teisendus, sagedustabeli leidmine.

Kui `DT` on *data.table* tüüpi andmetabel, siis paketi **data.table** põhisüntaksi kuju saab kirja panna järgnevalt
```{r, eval = F}
DT[i, j, by]
```
kus 

- `i` määrab read/objektid, mida edasi kasutada
- `j` määrab veerud, mis valitakse, uuendatakse või tekitatakse
- `by` määrab vajadusel grupitunnuse `j` tehtavateks arvutustesse.


Töölaual on andmestik `tekstid`, mis sisaldab tekstilõike ajalehe Postimees artiklitest. Kõigil lõikudel on ka emotsionaalne hinnang st kas lugejad on hinnanud lõigu negatiivseks, postiivseks, neutraalseks või vastuoluliseks(tunnus `hinnang`). Tekstilõigud on andmestikus tunnuse `tekst` nime all.

Andmed on pärit: http://peeter.eki.ee:5000/valence/paragraphsquery/



*** =instructions
- **Ülesanne 1** Kontrolli, kas andmestik on *data.table* tüüpi, kasutades funktsiooni `is.data.table()`. 
- **Ülesanne 2** Kasutades  **data.table** süntaksit, teisenda tunnus `loigunr`, mis on esialgu tüüpi `character`, arvuliseks, täpsemalt täisarvuks. Ära tekita uut tabelit, tee muudatus andmestiku `tekstid` sees.
- **Ülesanne 3** Kasutades **data.table** süntaksit, vali andmestikust `tekstid` need read, kus tegu on lõiguga, mille number on suurem kui 2 ja lõik algab tähega *A* (suurtäht), leia mitu sellist lõiku on igas lugejahinnangu `hinnang` grupis.  Omista tulemus muutujale `valik`. 

*** =hint
- Lõigu numbri tüübi teisendamisel pead tegema teisenduse andmestiku sees, st kasutama `:=` operaatorit kujul `loigunr := as.integer(loigunr)`.
- Kolmandas ülesandes saab sobivad read valida tingimusega `loigunr > 2 & startsWith(tekst, "A")`.


*** =pre_exercise_code
```{r}
library(data.table)
tekstid <- fread("http://kodu.ut.ee/~annes/R/tekstid.csv", nrow = 1000, encoding = "UTF-8", 
               col.names = c("rubriik", "artikkel", "loigunr", "hinnang", "tekst"))
tekstid[, artikkel := NULL]
n <-  (1:1000)[tekstid$loigunr == "None"]
tekstid <- tekstid[-n, ]
```

*** =sample_code
```{r}
# Ülesanne 1: kontrolli, kas andmestik on data.table-tüüpi
_______________________

# Ülesanne 2: tee tüübiteisendus
tekstid[_________]


# Ülesanne 3: vali read ja leia sagedused (ära pane veergudele uusi nimesid)
valik <- tekstid[___________]

```

*** =solution
```{r}
# Ülesanne 1: kontrolli, kas andmestik on data.table-tüüpi
is.data.table(tekstid)

# Ülesanne 2: tee tunnuse tüübiteisendus
tekstid[, loigunr := as.integer(loigunr)]


# Ülesanne 3: vali read ja leia sagedused (ära pane veergudele uusi nimesid)
valik <- tekstid[loigunr > 2 & startsWith(tekst, "A"), .N, by = hinnang]
valik

```

*** =sct
```{r}
#1
test_function("is.data.table",
              args = "x", index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Esimeses ülesandes kasuta funktsiooni `is.data.table()`.",
              args_not_specified_msg = "Funktsiooni `is.data.table()` argumendiks läheb andmestik.",
              incorrect_msg = "Funktsiooni `is.data.table()` argumendiks on vale andmestik.")

#2
test_function("as.integer",
              args = "x", index = 1,
              eval = FALSE,
              eq_condition = "equivalent",
              not_called_msg = "Teises ülesandes kasuta funktsiooni `as.integer()`.",
              args_not_specified_msg = "Funktsiooni `as.integer()` argumendiks läheb teisendatava tunnuse nimi.",
              incorrect_msg = "Funktsiooni `as.integer()` argumendiks on vale tunnus.")



test_student_typed("loigunr := " , not_typed_msg = "Kontrolli, kas teed teisenduse olemasoleva andmestiku sees st kas kasutad operaatorit `:=`")
test_student_typed(", loigunr ",  not_typed_msg = "Kontrolli, kas panid teisenduse kirja `j`-pesasse.")


test_data_frame("tekstid",
                columns = "loigunr",
                eq_condition = "equivalent",
                undefined_msg = "Andmestik `tekstid` on kadunud! Alusta uuesti.",
                undefined_cols_msg = "Veerg tekstilõigu numbriga on andmestikust kadunud.",
                incorrect_msg = "Veeru `loigunr` väärtus on vale.")


# 3

test_student_typed(", by = hinnang",  not_typed_msg = "Kontrolli, kas panid  `by`-pesasse kirja grupeeriva tunnuse nime. Jutumärke selle tunnuse nime ümber pole vaja!")



test_student_typed(".N, ", not_typed_msg = "Sageduse leidmiseks kirjuta `j`-pesasse: `.N` Ära praegu nimeta arvutatavat veergu ümber! ")




test_data_frame("valik",
                columns = c( "hinnang", "N"),
                eq_condition = "equivalent",
                undefined_msg = "Andmestikku `valik` pole! Aluta uuesti.",
                undefined_cols_msg = "Andmestikus `valik` pole kõiki veerge mis vaja.",
                incorrect_msg = "Andmestikus `valik` on mingid väärtused valed.")



                
                
success_msg("Väga tubli! Viimane ülesanne sai tehtud!")               
 


```
