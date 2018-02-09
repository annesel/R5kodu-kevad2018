---
title       : Pakett dplyr
description : Paketi dplyr kasutamine

--- type:NormalExercise lang:r xp:100 skills:1 key:818a1077ad
## Mitmele tunnusele kirjeldavate karakteristikute leidmine 2


Lisapaketis **dplyr** on olemas funktsioonide `mutate()` ja `summarise()` eriversioonid prefiksitega `_at`, `_if` ja `_all` juhuks kui funktsioone on vaja rakendada mingite tunnuste valikule nime järgi, loogilise tingimuse järgi või kõigile tunnustele, mis ei esine grupeeriva tunnusena. 



Töölaual on olemas andmestik `B`. Andmestikus on 160 inimese kohta mitmete testide ja ankeetküsitluse tulemused ning taustatunnuste väärtused.

Aktiveeritud on pakett **dplyr**.

Ülesandeks on sarnase vastustabeli moodustamine nagu eelmises ülesandes, nüüd aga paketi **dplyr** vahenditega.


*** =instructions
- **Ülesanne 1** Kasutades funktsioone `select()` ja `starts_with()` ning aheldamisoperaatorit `%>%` vali andmestikust need tunnused, mille nimi algab sõnaga *test*, omista valitud andmed muutujale `B1`.
- **Ülesanne 2** Täienda koodi kasutades funktsioone    `melt()`, `group_by()` ja `summarise_all()`, nii et tulemuseks oleks tabel kõigi *test*-tunnuste keskväärtuste, standardhälvete, miinimumide ja maksimumidega. Omista saadud tabel muutujale `tabel`.  Pane tähele, et funktsioon `melt()` ei ole **dplyr** paketi funktsioon.


*** =hint
- Aktiveerima peab paketi **reshape2**, et kasutada funktsiooni `melt()`.
- Kuna alamandmestikus `B1` on ainult testitulemused ja pole identifitseerivaid tunnuseid, siis andmestiku pikale kujule viimise võib kirja panna nii `B1 %>% melt()`. Tulemuseks on andmetabel kus ühes veerus on tunnuste nimed, teises tunnuste  väärtused.

*** =pre_exercise_code
```{r}
B <- read.csv2("http://kodu.ut.ee/~annes/R/B.csv", nrows = 160)
library(dplyr)

```

*** =sample_code
```{r}
# Vaata üle tunnusenimed ja andmestiku dimensioonid
names(B)
dim(B)

# Ülesanne 1: Alamandmestiku valik
B1 <- __________________________________



# Ülesanne 2: Täienda koodi
library(____________)
tabel <- B1 %>% __________________  %>%  _____________(___________)  %>%  ______________(.funs = c("mean", "sd", "min", "max"))
tabel[order(tabel$variable),]

```

*** =solution
```{r}
# Vaata üle tunnusenimed ja andmestiku dimensioonid
names(B)
dim(B)

# Ülesanne 1: Alamandmestiku valik
B1 <-  B %>% select(starts_with("test"))



# Ülesanne 2: Täienda koodi
library(reshape2)
tabel <- B1 %>% melt()  %>%  group_by(variable)  %>%  summarise_all(.funs = c("mean", "sd", "min", "max"))
tabel[order(tabel$variable),]
```

*** =sct
```{r}

#1
test_function("select",
              args = c(".data"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta esimeses ülesandes funktsiooni `select()`.",
              args_not_specified_msg = paste("Funktsioonis `select()` pead määrama  argumendi `.data`" ),
              incorrect_msg = paste("Funktsioonis `select()` on argumendi `.data` väärtus vale."))
       


test_function("starts_with",
              args = c("match"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta esimeses ülesandes funktsiooni `starts_with()`.",
              args_not_specified_msg = paste("Funktsioonis `starts_with()` pead määrama  argumendiks sobiva tunnusenime alguse jutumärkides." ),
              incorrect_msg = paste("Funktsioonis `starts_with()`  on  tunnusenime algus vale, kirjuta see samal kujul nagu ülesande tekstis."))
       

test_data_frame("B1",
                columns = NULL,
                eq_condition = "equivalent",
                undefined_msg = "Andmestikku `B1` pole tekitatud!",
                undefined_cols_msg = paste("Andmestikus `B1` on mõni nõutud veerg puudu! "),
                incorrect_msg = paste("Andmestikus `B1`  on mõni veerg valede väärtustega!"))
                
                


# 2
test_function(name = "library", 
              args = "package",
              index = 1,
              eval = FALSE,
              eq_condition = "equivalent",
              not_called_msg = "Esimeses ülesandes pead kasutama funktsiooni `library()`, et aktiveerida pakett **reshape2**.",
              args_not_specified_msg = "Käsule `library()` tuleb argumendiks anda paketi nimi.",
              incorrect_msg =  "Käsu `library()` argumendiks anna paketi nimi  `reshape2`")



test_function("melt",
              args = c("data"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta teises ülesandes funktsiooni `melt()`.",
              args_not_specified_msg = paste("Funktsioonile `melt()` peab aheldamise kaudu argumendiks minema andmestik `B1`." ),
              incorrect_msg = paste("Funktsioonile `melt()` saadetakse argumendiks  vale andmestik."))
       




test_function("group_by",
              args = c(".data"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta teises ülesandes funktsiooni `group_by()`.",
              args_not_specified_msg = paste("Funktsiooni `group_by()` esimeseks argumendiks peab aheldamine saatma pikas formaadis andmestiku." ),
              incorrect_msg = paste("Funktsiooni `group_by()`  rakendatakse valele andmestikule. "))
       

 



test_function("summarise_all",
              args = c(".tbl", ".funs"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta teises ülesandes funktsiooni `summarise_all()`.",
              args_not_specified_msg = paste("Funktsiooni `summarise_all()`", 
                                c(" esimeseks argumendiks peab sattuma grupeeritud andmestik.", "teiseks arumendiks peab olema vektor funktsioonide nimedega.") ),
              incorrect_msg = paste("Funktsiooni `summarise_all()`  ",  c(" rakendatakse  valele andmestikule.", " funktsioonide nimekiri pole sama, mis etteantud koodis. Ära kustuta, ega muuda seda kohta koodist.")   ))
 
 
 
 
test_pipe(num = 4, absent_msg = "Kasuta aheldamisoperaatorit `%>%`.", insuf_msg = "Kasuta aheldamisoperaatorit `%>%` vähemalt 4 korda.") 
 
 
 
test_data_frame("tabel",
                columns = c("variable", "mean", "sd", "min", "max"),
                eq_condition = "equivalent",
                undefined_msg = "Tabelit `tabel` pole tekitatud!",
                undefined_cols_msg = paste("Tabelis `tabel` on mõni nõutud veerg puudu! "),
                incorrect_msg = paste("Tabelis `tabel` on mõni veerg valede väärtustega!"))
                
                
                
                
                
                
                
success_msg("Ülesanne tehtud. Väga tubli!")               
       

```










--- type:NormalExercise lang:r xp:100 skills:1 key:edea2d2fcc
## Teisenduse määramine mitmele tunnusele andmestikus

Lisapaketis **dplyr** on olemas funktsioonide `mutate()` ja `summarise()` eriversioonid sufiksitega `_at`, `_if` ja `_all` juhuks kui fuktsioone on vaja rakendada mingite tunnuste valikule nime järgi, loogilise tingimuse järgi või kõigile tunnustele, mis ei esine grupeeriva tunnusena. 



Töölaual on olemas andmestikud `mass` (tuttav praktikumist) ja `antropo`. Andmestikus  `antropo` on mõned antropomeetrilised mõõtmised, mõõdetud on  pikkus õlani (mm), kerepikkus istudes (mm),  põlve kõrgus, istudes (mm),  talje ümbermõõt (mm),  õlgade ümbermõõt (mm),  randmeümbermõõt (mm) ja  kaal (kg*10), lisaks on teada uuritava sugu.

Aktiveeritud on pakett **dplyr**.

 


*** =instructions
- **Ülesanne 1**  Teisenda andmestikus `mass` kõik tunnused, mis on tüüpi `factor` tavaliseks tekstiks ehk tüüpi `character`. Selleks täienda antud koodi sobivalt. Teisendatud tunnustega andmestik nimeta `mass_char`. Koodi kirjapanekul kasuta aheldamisoperaatorit `%>%`.
- **Ülesanne 2** Teisenda andmestikus `antropo` kõik mõõtmised, mis on millimeetrites sentimeetriteks ja tunnus, mille mõõtühik on 10*kg, kilogrammidesse. Defineeri selleks uus funktsioon, mida saad edasi kasutada sobiva sufiksiga `mutate_`-käsus. Teisendus tegemiseks täienda antud koodi. Täiendamisel kasuta aheldamisoperaatorit `%>%`, muudetud andmestik omista muutujale `antropo_cm_kg`. 


*** =hint
- Selleks, et kontrollida, kas tunnus on faktortüüpi, saab kasutada funktsiooni `is.factor()`.
- Selleks, et tunnuse tüüpi määrata tavaliseks tekstitüübiks, kasuta funktsiooni `as.character()`.
- Uus funktsioon peaks läbi viima jagamise arvuga 10, `function(x) x/10`.

*** =pre_exercise_code
```{r}
antropo <- read.table("http://kodu.ut.ee/~annes/R/antropo.txt",header=T,sep="\t")
mass<- read.table("http://kodu.ut.ee/~annes/Rkursus/mass.txt",header=T,sep="\t")

library(dplyr)

```

*** =sample_code
```{r}
# Tutvu andmestikega
str(mass)
str(antropo)

# Ülesanne 1: teisenda faktorid tavaliseks tekstiks
_________ <-  __________ %>%  mutate______(_________________)


# Ülesanne 2: teisenda ühikud
uus_funktsioon <- function(_____) ____________
antropo_cm_kg <- _______  ____ mutate______(vars(-SEX), .funs = uus_funktsioon)

```

*** =solution
```{r}
# Tutvu andmestikega
str(mass)
str(antropo)

# Ülesanne 1: teisenda faktorid tavaliseks tekstiks
mass_char <-  mass %>%  mutate_if(.predicate = is.factor, .funs = as.character)


# Ülesanne 2: teisenda ühikud
uus_funktsioon <- function(x) x/10
antropo_cm_kg <- antropo  %>% mutate_at(vars(-SEX), .funs = uus_funktsioon)


```

*** =sct
```{r}
# 1
test_function("mutate_if",
              args = c(".tbl", ".predicate", ".funs"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta esimeses ülesandes funktsiooni `mutate_if()`.",
              args_not_specified_msg = paste("Funktsiooni `mutate_if()`", 
                                c(" esimeseks argumendiks peab sattuma andmestik `mass`, st see saadetakse aheldamisega funktsiooni esimeseks argumendiks.", 
                                " peab määrama loogilise kontrolli, millele vastavatele veergudele hakatakse teisendust tegema. Kontrollima peab, kas veerus on `factor`-tüüpi tunnus.", 
                                " peab määrama teisenduse, mida valitud veergudele rakendada, see peaks veeru tüübiks määrama `character`") ),
              incorrect_msg = paste("Funktsiooni `mutate_if()`  ",  c(" rakendatakse  valele andmestikule.", 
              " on antud vale tingimus, millele veerud peavad vastama.", 
              " on määratud vale teisendus.")   ))
 

 
 
  
test_data_frame("mass_char",
                columns = NULL,
                eq_condition = "equivalent",
                undefined_msg = "Andmestikku `mass_char` pole tekitatud!",
                undefined_cols_msg = paste("Andmestikus `mass_char` on mõni veerg puudu! "),
                incorrect_msg = paste("Andmestikus `mass_char` on mõni veerg valede väärtustega!"))
                
                
 
 
#2
test_function_definition("uus_funktsioon",
                         function_test = test_expression_result("uus_funktsioon(c(Inf, -Inf, 0, -5, 1, 2))"),
                         body_test = NULL,
                         undefined_msg = "Funktsioon `uus_funktsioon` on defineerimata.",
                         incorrect_number_arguments_msg = "Piisab, kui defineeritaval funkstioonil on üks argument.")




test_function("mutate_at",
              args = c(".tbl", 
              #".vars", 
              ".funs"), index = 1,
              eval = c(TRUE,   TRUE),
              eq_condition = "equivalent",
              not_called_msg = "Kasuta Esimeses ülesandes funktsiooni `mutate_at()`.",
              args_not_specified_msg = c("Funktsiooni `mutate_at()` esimeseks argumendiks peab sattuma andmestik `mass`, st see saadetakse aheldamisega funktsiooni esimeseks argumendiks.", 
                               # " Ära kustuta funktsiooni `mutate_at()` etteantud argumentide väärtusi.", 
                                " Ära kustuta funktsiooni `mutate_at()` etteantud argumentide väärtusi.") ,
              incorrect_msg = c("Funktsiooni `mutate_at()`   rakendatakse  valele andmestikule.", 
              #"Ära muuda funktsiooni `mutate_at()at()` etteantud argumentide väärtusi.", 
              "Ära muuda funktsiooni `mutate_at()` etteantud argumentide väärtusi, kirjapilti.")   )

			  
			  
			  
			  

test_pipe(num = 2, absent_msg = "Kasuta aheldamisoperaatorit `%>%`.", insuf_msg = "Kasuta aheldamisoperaatorit `%>%` vähemalt 2 korda.") 
 
 
 
test_data_frame("antropo_cm_kg",
                columns = NULL,
                eq_condition = "equivalent",
                undefined_msg = "Andmestikku `antropo_cm_kg` pole tekitatud!",
                undefined_cols_msg = paste("Andmestikus `antropo_cm_kg` on mõni veerg puudu! "),
                incorrect_msg = paste("Andmestikus `antropo_cm_kg` on mõni veerg valede väärtustega!"))
                
                
 
                
                
                
                
                
                
success_msg("Ülesanne tehtud. Tubli!")               
       





```

--- type:NormalExercise lang:r xp:100 skills:1 key:cfbdcdcc57
## Andmestike ühendamine



Paketis **dplyr** on olemas valik funktsioone, mis on mõeldud andmestike ühendamiseks:

* `inner_join()`
* `left_join()`
* `right_join()`
* `full_join()`
* `semi_join()`
* `anti_join()`

Selles ülesandes tuleb valida sobiv funktsioon ülaltoodud nimekirjast.


Töölaual on olemas kaks andmestikku: 

- andmestikus `A` on kirjas vastajate id-kood, sugu, elukoht, vanus, pikkus, kaal, käte siruulatus ning arstivisiidi toimumine.
- andmestikus `B` on kirjas vastajate id-kood, uuringugrupi tunnus ning vastused mitmesugustele testidele.

Pakett **dplyr** on juba aktiveeritud.




*** =instructions
- **Ülesanne 1** Teisenda esmalt mõlemas andmestikus faktortunnused tavaliseks tekstiks, selleks täienda etteantud koodi. Teisendatud andmestikud omista muutujatele `A1` ja `B1`. Edasi kasuta neid andmestikke.

- **Ülesanne 2** Ühenda andmestikud id-koodi tunnuse põhjal, nimeta ühendatud andmestik nimega `AB1`. Tulemuseks olevas andmestikus peaks olema ainult need uuritavad, kelle kohta on olemas info mõlemas andmestikus, kuid veergudest ainult need, mis on olemas andmestikus `B`. Ühendamise läbiviimiseks vali üks `_join()` funktsioon ülaltoodud nimekirjast. Koodi kirjapanekul kasuta `%>%` operaatorit.

- **Ülesanne 3** Ühenda andmestikud id-koodi tunnuse põhjal, nimeta ühendatud andmestik nimega `AB2`. Tulemuseks olevas andmestikus peaks olema ainult need uuritavad, kes on andmestikus `A` ja kellel on vaste andmestikus `B`ning  kõik veerud mõlemast andmestikust.
 Ühendamise läbiviimiseks vali üks `_join()` funktsioon ülaltoodud nimekirjast. Koodi kirjapanekul kasuta `%>%` operaatorit.

 



*** =hint
-  
-  

*** =pre_exercise_code
```{r}
A <- read.csv2("http://kodu.ut.ee/~annes/R/A.csv", nrows = 45)
B <- read.csv2("http://kodu.ut.ee/~annes/R/B.csv", nrows = 160)
B <- B[, c("id", "grupp", sort(names(B)[-(1:2)]))]

library(dplyr)

```

*** =sample_code
```{r}
# Vaata anmdestikud üle
str(A)
str(B)


# Ülesanne 1:  Teisenda tunnuse tüüp
A1 <- A %>% mutate_if(__________)
B1 <- B %>% mutate_if(__________)


# Ülesanne 2: ühenda andmestikud
AB1 <- ___ %>% _________________

# Ülesanne 3: ühenda andmestikud
AB2 <- ___ %>% _________________


```

*** =solution
```{r}
# Vaata andmestikud üle
str(A)
str(B)


# Ülesanne 1:  Teisenda tunnuse tüüp
A1 <- A %>% mutate_if(is.factor, as.character)
B1 <- B %>% mutate_if(is.factor, as.character)


# Ülesanne 2: ühenda andmestikud
AB1 <- B1 %>% semi_join(A1, by = "id")

# Ülesanne 3: ühenda andmestikud
AB2 <- A1 %>% inner_join(B1, by = "id")


```



*** =sct
```{r}
test_predefined_objects("A", 
                        eq_condition = "equivalent",
                        eval = TRUE,
                        undefined_msg = "Ära kustuta andmestikku `A`.", 
                        incorrect_msg = "Ära muuda andmestiku `A` sisu.")
 

test_predefined_objects("B", 
                        eq_condition = "equivalent",
                        eval = TRUE,
                        undefined_msg = "Ära kustuta andmestikku `B`.", 
                        incorrect_msg = "Ära muuda andmestiku `B` sisu.")


# 1
test_function(name = "mutate_if",
              args = c(".predicate", ".funs"),
              index = 1,
              eval = TRUE,
             eq_condition = "equivalent",
             not_called_msg = "Pead ülesandes  1  kasutama funktsiooni `mutate_if()`",
             args_not_specified_msg = paste("Määra `mutate_if()` käsus argumendi", c("`.predicate`", "`.funs`" ), "väärtus."),
             incorrect_msg = paste("Muuda `mutate_if()` käsus argumendi", c("`.predicate`", "`.funs`"), "väärtust, see pole praegu õige.")) 

test_function(name = "mutate_if",
              args = c(".predicate", ".funs"),
              index = 2,
              eval = TRUE,
             eq_condition = "equivalent",
             not_called_msg = "Pead ülesandes  1  kasutama funktsiooni `mutate_if()`",
             args_not_specified_msg = paste("Määra `mutate_if()` käsus argumendi", c("`.predicate`", "`.funs`" ), "väärtus."),
             incorrect_msg = paste("Muuda `mutate_if()` käsus argumendi", c("`.predicate`", "`.funs`"), "väärtust, see pole praegu õige.")) 

 
 

# 2
test_function(name = "semi_join",
              args = c("x", "y", "by"),
              index = 1,
              eval = TRUE,
             eq_condition = "equivalent",
             not_called_msg = "Pead teises ülesandes    kasutama funktsiooni `semi_join()`",
             args_not_specified_msg = paste("Käsus `semi_join()`  ", c("saadetakse esimene liidetav andmestik läbi aheldamisoperaatori", " tuleb märkida teine liidetav andmestik", "tuleb määrata võtmetunnus `by`", "." )),
             incorrect_msg = paste("Muuda `semi_join()` käsus ", c("läbi aheldamise saadetav andmestik.", " teine liidetav andmestik. ", "argumendi `by` väärtus."))) 


test_data_frame("AB1",
                columns = NULL,
                eq_condition = "equivalent",
                undefined_msg = "Andmestikku `AB1` pole tekitatud!",
                undefined_cols_msg = paste("Andmestikus `AB1` on mõni veerg puudu! "),
                incorrect_msg = paste("Andmestikus `AB1` on mõni veerg valede väärtustega!"))
                
                
 




# 3
test_or(
test_function(name = "inner_join",
              args = c("x", "y", "by"),
              index = 1,
              eval = TRUE,
             eq_condition = "equivalent",
             not_called_msg = "Pead kolmandas ülesandes  teises  funktsiooni `inner_join()`.",
             args_not_specified_msg = paste("Käsus `inner_join()`  ", c("saadetakse esimene liidetav andmestik läbi aheldamisoperaatori", " tuleb märkida teine liidetav andmestik", "tuleb määrata võtmetunnus `by`", "." )),
             incorrect_msg = paste("Muuda `inner_join()` käsus ", c("läbi aheldamise saadetav andmestik.", " teine liidetav andmestik. ", "argumendi `by` väärtus.")) )
,
test_function(name = "inner_join",
              args = c("by"),
              index = 1,
             eval = TRUE,
             eq_condition = "equivalent",
             not_called_msg = "Pead kolmandas ülesandes  teises  funktsiooni `inner_join()`",
             args_not_specified_msg = paste("Käsus `inner_join()`  tuleb määrata võtmetunnus `by`.", "." ),
             incorrect_msg = paste("Muuda `inner_join()` käsus argumendi `by` väärtus.")) 
)


test_data_frame("AB2",
                columns = NULL,
                eq_condition = "equivalent",
                undefined_msg = "Andmestikku `AB2` pole tekitatud!",
                undefined_cols_msg = paste("Andmestikus `AB2` on mõni veerg puudu! "),
                incorrect_msg = paste("Andmestikus `AB2` on mõni veerg valede väärtustega!"))
                
                
 
 test_pipe(num = 4, absent_msg = "Kasuta aheldamisoperaatorit `%>%`.", insuf_msg = "Kasuta aheldamisoperaatorit `%>%` vähemalt 4 korda.") 
 
            
            
            
            
success_msg("Hästi!")

```
