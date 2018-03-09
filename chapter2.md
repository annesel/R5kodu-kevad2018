---
title       : Pakett data.table
description : Andmetöötluspaketi data.table kasutamine






--- type:NormalExercise lang:r xp:100 skills:1 key:62a4ef70a0
## Ridade filtreerimine ja veergude valik/defineerimine

Kui `DT` on *data.table* tüüpi andmetabel, siis paketi **data.table** põhisüntaksi kuju saab kirja panna järgnevalt
```{r, eval = F}
DT[i, j, by]
```
kus 

- `i` määrab read/objektid, mida edasi kasutada
- `j` määrab veerud, mis valitakse, uuendatakse või tekitatakse
- `by` määrab vajadusel grupitunnuse `j` tehtavateks arvutusteks.

Töölaual on olemas andmestik `A`. Andmestikus on kirjas 40 inimese id-kood, sugu, elukoht, vanus, pikkus, kaal, käte siruulatus ning arstivisiidi toimumine.

 
Ülesannetes nõutud arvutustes ära kasuta ümardamist.



*** =instructions
- **Ülesanne 1** Aktiveeri pakett **data.table**.
- **Ülesanne 2** Kasutades **data.table** süntaksit, tekita tabel `tabel1`, kus on näha üle 50 aastaste ja üle 80 kg kaaluvate uuritavate KMI väärtus (tunnus `kmi`) ja käte siruulatus (tunnus `sirutus`). Prindi tulemus ekraanile.
- **Ülesanne 3** Kasutades **data.table** süntaksit, leia soo ja elukoha gruppides keskmine vanus (`kesk.vanus`) ja keskmine pikkus (`kesk.pikkus`) neile, kes pole käinud arstivisiidil (tunnuse `visiit` väärtus on `FALSE`). Määra grupeering nii, et tulemustabelis on soo tunnus esimene veerg ning elukoht teine. Prindi tulemus ekraanile.

*** =hint
- Teises ülesandes peab KMI väärtuse arvutama ja omistama veergu nimega `kmi`, teise tunnuse `sirutus` väärtustega mingit teisendust pole vaja teha. Arvutatav tunnus ja valitud veeru nimi peab olema antud listina:  `.(kmi = _____, sirutus)` või `list(kmi = _____, sirutus)`.
- Kolmandas ülesandes on vaja määrata kaks grupeerivat tunnust, kasuta ka siin listi.



*** =pre_exercise_code
```{r}
A0 <- read.csv2("http://kodu.ut.ee/~annes/R/A.csv", nrows = 45)
A <- data.table::as.data.table(A0)
rm(A0)

```

*** =sample_code
```{r}
# Ülesanne 1: aktiveeri pakett
_______________________


# Ülesanne 2: tee valik objektidest ja vali/arvuta tunnused
tabel1 <- A[______, ______]
______

# Ülesanne 3: tee valik objektidest ja vali/arvuta tunnused gruppide kaupa
tabel2 <- A[______, ______, ______]
______

```

*** =solution
```{r}
# Ülesanne 1: aktiveeri pakett
library(data.table)

# Ülesanne 2: tee valik objektidest ja vali/arvuta tunnused
tabel1 <- A[vanus > 50 & kaal > 80, .(kmi = kaal/(kasv/100)^2, sirutus )]
tabel1

# Ülesanne 3: tee valik objektidest ja vali/arvuta tunnused  tunnused gruppide kaupa
tabel2 <-A[visiit == FALSE, .(kesk.vanus = mean(vanus), kesk.pikkus = mean(kasv)), by = .(sugu, elukoht)]
tabel2

```

*** =sct
```{r}
#1
test_function(name = "library", 
              args = "package",
              index = 1,
              eval = FALSE,
              eq_condition = "equivalent",
              not_called_msg = "Esimeses ülesandes pead kasutama funktsiooni `library()`.",
              args_not_specified_msg = "Käsule `library()` tuleb argumendiks anda paketi nimi.",
              incorrect_msg =  "Käsu `library()` argumendiks anna paketi nimi  `data.table`")


#2
test_data_frame("tabel1", columns = c("kmi", "sirutus"),
            undefined_msg = "Andmetabel `tabel1` on defineerimata.",
            undefined_cols_msg = paste("Andmestikus `tabel1` on mingi veerg puudu, võibolla on veeru nimi vale."),
            incorrect_msg = "Andmetabelis `tabel1` on mingi veeru väärtused valed või on veeru nimi vale. Proovi uuesti." )

test_output_contains("tabel1", "Esimene tabel on ekraanile printimata")





 
#2
test_or(
test_student_typed(", by = .(sugu, elukoht)",  not_typed_msg = "Kontrolli, kas panid  `by`-pesasse kirja grupeerivate tunnuste nimed listina."),
test_student_typed(", by = list(sugu, elukoht)",  not_typed_msg = "Kontrolli, kas panid  `by`-pesasse kirja grupeerivate tunnuste nimed listina.")
)

 


test_data_frame("tabel2", columns = c("sugu", "elukoht", "kesk.vanus", "kesk.pikkus"),
            undefined_msg = "Andmetabel `tabel2` on defineerimata.",
            undefined_cols_msg = paste("Andmestikus `tabel2` on mingi veerg puudu, võibolla on veeru nimi vale."),
            incorrect_msg = "Andmetabelis `tabel1` on mingi veeru väärtused valed või on veeru nimi vale. Proovi uuesti." )

test_output_contains("tabel2", "Teine tabel on ekraanile printimata")

 

 
```


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

Pakett **data.table** on juba aktiveeritud.

*** =instructions
- **Ülesanne 1** Kontrolli, kas andmestik on *data.table* tüüpi, kasutades funktsiooni `is.data.table()`. 
- **Ülesanne 2** Kasutades  **data.table** süntaksit, teisenda tunnus `loigunr`, mis on esialgu tüüpi `character`, arvuliseks, täpsemalt täisarvuks. Ära tekita uut tabelit, tee muudatus andmestiku `tekstid` sees.
- **Ülesanne 3** Kasutades **data.table** süntaksit, vali andmestikust `tekstid` need read, kus tegu on lõiguga, mille number on suurem kui 2 ja lõik algab tähega *A* (suurtäht), leia mitu sellist lõiku on igas lugejahinnangu `hinnang` grupis.  Omista tulemus muutujale `valik`. 

*** =hint
- Lõigu numbri tüübi teisendamisel pead tegema teisenduse andmestiku sees, st kasutama `:=` operaatorit kujul `loigunr := as.integer(loigunr)`.
- Kolmandas ülesandes saab sobivad read valida näiteks tingimusega `loigunr > 2 & startsWith(tekst, "A")`.


*** =pre_exercise_code
```{r}
library(data.table)
tekstid <- fread("http://kodu.ut.ee/~annes/R/tekstid.csv", nrow = 1000, encoding = "UTF-8", 
               col.names = c("rubriik", "artikkel", "loigunr", "hinnang", "tekst"))
tekstid[, artikkel := NULL]
n <-  (1:1000)[tekstid$loigunr == "None"]
tekstid <- tekstid[-n, ]
rm(n)
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
                undefined_msg = "Andmestikku `valik` pole! Alusta uuesti.",
                undefined_cols_msg = "Andmestikus `valik` pole kõiki veerge mis vaja.",
                incorrect_msg = "Andmestikus `valik` on mingid väärtused valed.")



                
                
success_msg("Väga tubli! Viimane ülesanne sai tehtud!")               
 


```










