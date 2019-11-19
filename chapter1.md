---
title: 'Pakett dplyr'
description: 'Paketi dplyr kasutamine'
---

## Andmestikku tunnuste lisamine

```yaml
type: NormalExercise
key: 49f706f3a3
lang: r
xp: 100
skills: 1
```

Paketi **dplyr** funktsioonidest `mutate()` on abiks kui on vaja andmestikku uusi tunnuseid lisada. Korraga saab defineerida mitu uut tunnust, kusjuures tunnust, mille definitsioon on käsus juba kirjas, saab kohe samas kasutada.<br><br>


Töölaual on olemas andmestik `A`. Andmestikus on kirjas 40 inimese id-kood, sugu, elukoht, vanus, pikkus, kaal, käte siruulatus ning arstivisiidi toimumine.

`@instructions`
- **Ülesanne 1:** Aktiveeri pakett **dplyr**.
- **Ülesanne 2:** Kasutades paketi **dplyr** funktsiooni `mutate()`, lisa andmestikku uuritavate kehamassiindeksi  väärtus ehk KMI (tunnus nimega `kmi`) ja tunnus, mis sellesama indeksi põhjal jagab inimesed 2 gruppi: kui KMI on kuni 25 (kaasa arvatud), siis on grupitunnuse väärtus `ala voi normkaal`, kui KMI on üle 25, siis `ylekaal`. Grupitunnuse nimeks vali `kaalugrupp`, selle moodustamiseks kasuta funktsiooni `ifelse()`, tõeväärtusvektor tekita nii, et väärtus `TRUE` vastaks madalamale KMI väärtusele.
- **Ülesanne 3:** Vaata üle uue andmestiku struktuur käsuga `str()`.

`@hint`
- Kui kirjutad `mutate()` käsku uute tunnuste definitsioonid, siis vaata, et KMI arvutus eelneks kaalugruppide määramisele, sest siis on KMI väärtust juba vaja kasutada.
- KMI  arvutusvalem: (kaal kilogrammides) / (pikkus meetrites)^2

`@pre_exercise_code`
```{r}
A <- read.csv2("http://kodu.ut.ee/~annes/R/A.csv", nrows = 45)

```

`@sample_code`
```{r}
# Vaata andmestik üle
head(A)


# Ülesanne 1: aktiveeri pakett
_______(______)


# Ülesanne 2: lisa tunnused
A1 <- mutate(_____, kmi = _____, kaalugrupp = ifelse(_____, _____, _____))


# Ülesanne 3: vaata tulemust
______(_____)


```

`@solution`
```{r}
# Vaata andmestik üle
head(A)


# Ülesanne 1: aktiveeri pakett
library(dplyr)
 


# Ülesanne 2: lisa tunnused
A1 <- mutate(A, 
            kmi = kaal/(kasv/100)^2, 
            kaalugrupp = ifelse(kmi <= 25, "ala voi normkaal", "ylekaal"))


# Ülesanne 3: vaata tulemust
str(A1)
```

`@sct`
```{r}
# 1
test_function(name = "library", 
              args = "package",
              index = 1,
              eval = FALSE,
              eq_condition = "equivalent",
              not_called_msg = "Esimeses ülesandes pead kasutama funktsiooni `library()`.",
              args_not_specified_msg = "Käsule `library()` tuleb argumendiks anda paketi nimi.",
              incorrect_msg =  "Käsu `library()` argumendiks anna paketi nimi  `dplyr`")



# 2
test_function(name = "mutate", 
              args = ".data",
              index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Teises ülesandes pead kasutama funktsiooni `mutate()`.",
              args_not_specified_msg = "Käsule `mutate()` tuleb  esimeseks argumendiks anda andmestiku nimi `A`.",
              incorrect_msg =  "Käsule `mutate()` antud andmestik on vale.")




test_function(name = "ifelse", 
              args = c("test", "yes", "no"),
              index = 1,
              eval = c(F, T, T),
              eq_condition = "equivalent",
              not_called_msg = "Teises ülesandes pead kasutama funktsiooni `ifelse()`.",
              args_not_specified_msg = paste("Käsu `ifelse()` argumentidest peab " ,
                                c("esimene olema loogiline test", 
                                "teine olema  määratav väärtus, kui loogilise testi tulemus on `TRUE`", 
                                "kolmas olema `FALSE` tulemuse korral määratav väärtus."))
                                ,
              incorrect_msg =  paste("Käsule `ifelse()` on antud vale väärtus ", c("esimeseks", "teiseks", "kolmandaks"), " argumendiks.")   )




test_data_frame("A1", columns = c("kmi", "kaalugrupp"),
            undefined_msg = "Andmetabel `A1` on defineerimata.",
            undefined_cols_msg = paste("Andmestikus `A1` on mingi veerg puudu, võibolla on veeru nimi vale."),
            incorrect_msg = "Andmetabelis `A1` on mingi veeru väärtused valed või on veeru nimi vale. Proovi uuesti." )

 

# 3
test_function(name = "str", 
              args = "object",
              index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Viimases ülesandes pead kasutama funktsiooni `str()`.",
              args_not_specified_msg = "Käsule `str()` tuleb argumendiks anda andmestiku nimi.",
              incorrect_msg =  "Käsu `str()` argumendiks on vale andmestik.")

			  
			  

success_msg("Tubli!")               
       
            

###########################################################
```

---

## Grupikokkuvõtete arvutamine

```yaml
type: NormalExercise
key: d7c62aafff
lang: r
xp: 100
skills: 1
```

Paketi **dplyr** funktsiooni `summarise()` saab kasutada andmete agregeerimiseks. <br><br>

Töölaual on olemas andmestik `A1` eelmisest ülesandest. Andmestikus on kirjas 40 inimese id-kood, sugu, elukoht, vanus, pikkus, kaal, käte siruulatus ning arstivisiidi toimumine. Lisatud on KMI ja kaalugrupi tunnus. 


Aktiveeritud on pakett **dplyr**.

`@instructions`
- **Ülesanne 1:** Kasutades funktsiooni `summarise()` leia tabel, kus soo ja elukoha gruppides oleks esitatud uuritavate arv (`n`), keskmine vanus (`kesk.vanus`), keskmine KMI (`kesk.kmi`) ja  arstivisiidil käinute osakaal (`visiit.osak`). Tulemuseks olevas tabelis peaks esimeseks veeruks olema soo tunnus, teiseks elukoht.  Koodi kirjapanekul kasuta aheldamisoperaatorit `%>%`.
- **Ülesanne 2:** Milline grupp on kõige madalama arstivisiidil käinute osakaaluga? Omista selle grupi koodid (0 või 1)  vastusesse.

`@hint`
- Kasuta funktsiooni `group_by()`, et määrata andmestikule grupeering soo ja elukoha põhjal

`@pre_exercise_code`
```{r}
library(dplyr)
A <- read.csv2("http://kodu.ut.ee/~annes/R/A.csv", nrows = 45)
A1 <- mutate(A, kmi = kaal/(kasv/100)^2, kaalugrupp = ifelse(kmi <= 25, "ala- või normaalkaal", "ylekaal"))
rm(A)
```

`@sample_code`
```{r}
# Vaata andmestik üle
str(A1)

# Ülesanne 1: leia tabel
tabel <- A1 %>%   
            _______(_____, _____) %>% 
                    ________(n = ______, kesk.vanus = _____, kesk.kmi = _____, visiit.osak = _____)
tabel

# Ülesanne 2: leia mis grupp on kõige madalama arstivisiidil käimise osakaaluga.
madal <- list(sugu = _____, elukoht = _____)
```

`@solution`
```{r}
# Vaata andmestik üle
str(A1)

# Ülesanne 1: leia tabel
tabel <- A1 %>% 
            group_by(sugu, elukoht) %>% 
                summarise(n = length(sugu),
                kesk.vanus = mean(vanus), kesk.kmi = mean(kmi), 
                visiit.osak = sum(visiit)/length(visiit))
tabel

# Ülesanne 2: leia mis grupp on kõige madalama arstivisiidil käimise osakaaluga.
madal <- list(sugu = 1, elukoht = 0)
```

`@sct`
```{r}

# 1
test_function("group_by",
              args = c(".data"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Esimeses ülesandes peab andmestiku grupeerima enne kui hakata arvutusi tegema. Kasuta selleks `group_by()` funktsiooni.",
              args_not_specified_msg = paste("Funktsiooni `group_by()` esimeseks argumendiks peab aheldamine saatma  andmestiku `A1`. Grupeerivad tunnused anna enne järjekorras: `sugu, elukoht`" ),
              incorrect_msg = paste("Funktsiooni `group_by()`  rakendatakse valele andmestikule. "))
       

 

test_function("summarise",
              args = c(".data"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta esimeses ülesandes funktsiooni `summarise()`.",
              args_not_specified_msg = paste("Funktsiooni `summarise()`", 
                                c(" esimeseks argumendiks peab sattuma grupeeritud andmestik.") ),
              incorrect_msg = paste("Funktsiooni `summarise()`  ",  c(" rakendatakse  valele andmestikule.")   ))
 


test_pipe(num = 2, absent_msg = "Kasuta aheldamisoperaatorit `%>%`.", insuf_msg = "Kasuta aheldamisoperaatorit `%>%` vähemalt 2 korda.") 
  
       
test_data_frame("tabel",
                columns = c("sugu", "elukoht",  "n",  "kesk.vanus", "kesk.kmi", "visiit.osak"),
                eq_condition = "equivalent",
                undefined_msg = "Tabelit `tabel` pole tekitatud!",
                undefined_cols_msg = paste("Tabelis `tabel` on mõni nõutud veerg puudu või on mõni veeru nimi vale.  Kontrolli ka, kas andmestiku grupeerimine käis nõutud järjekorras: `group_by(sugu, elukoht)`."),
                incorrect_msg = paste("Tabelis `tabel` on mõni veerg valede väärtustega või on mõni veeru nimi vale.  Kontrolli ka, kas andmestiku grupeerimine käis nõutud järjekorras: `group_by(sugu, elukoht)`."))
                
                
 

# 2
test_object("madal",
            eq_condition = "equivalent",
            eval = TRUE,
            undefined_msg = "List `madal` on tekitamata.",
            incorrect_msg = "Listis `madal` on mingi väärtus vale.")






success_msg("Väga tubli!")               
       
            

###########################################################
```

---

## Mitmele tunnusele kirjeldavate karakteristikute leidmine

```yaml
type: NormalExercise
key: 818a1077ad
lang: r
xp: 100
skills: 1
```

Lisapaketis **dplyr** on olemas funktsioonide `mutate()` ja `summarise()` eriversioonid prefiksitega `_at`, `_if` ja `_all` juhuks kui funktsioone on vaja rakendada mingite tunnuste valikule nime järgi, loogilise tingimuse järgi või kõigile tunnustele, mis ei esine grupeeriva tunnusena. 


Töölaual on olemas andmestik `B`. Andmestikus on 160 inimese kohta mitmete testide ja ankeetküsitluse tulemused ning taustatunnuste väärtused.

Aktiveeritud on pakett **dplyr**.

Ülesandeks on moodustada tabel, kus on igale testitulemusele leitud keskväärtus, standardhälve ning miinimum ja maksimum. Tabelis peaks iga testitunnus määrama ühe rea, mis on täidetud tema karakteristikutega:
```{r}
   variable     mean       sd   min   max
 1  test109 19.82375 2.034034  14.0  26.3
 2  test141 20.14250 2.173267  14.3  26.5
 3  test113 19.94500 2.157940  13.9  25.1
 4  test177 19.84625 1.958754  15.3  24.7
# ... jne
```

`@instructions`
- **Ülesanne 1:** Kasutades funktsioone `select()` ja `starts_with()` ning aheldamisoperaatorit `%>%` vali andmestikust  `B` need tunnused, mille nimi algab sõnaga *test*, omista valitud andmed muutujale `B1`.
- **Ülesanne 2:** Täienda koodi kasutades funktsioone  `melt()`, `group_by()` ja `summarise_all()`, nii et tulemuseks oleks tabel kõigi *test*-tunnuste keskväärtuste, standardhälvete, miinimumide ja maksimumidega. Omista saadud tabel muutujale `tabel`.  Pane tähele, et funktsioon `melt()` ei ole **dplyr** paketi funktsioon.

`@hint`
- Aktiveerima peab paketi **reshape2**, et kasutada funktsiooni `melt()`.
- Kuna alamandmestikus `B1` on ainult testitulemused ja pole identifitseerivaid tunnuseid, siis andmestiku pikale kujule viimise võib kirja panna nii `B1 %>% melt()`. Tulemuseks on andmetabel, kus ühes veerus on tunnuste nimed, teises tunnuste  väärtused.

`@pre_exercise_code`
```{r}
B <- read.csv2("http://kodu.ut.ee/~annes/R/B.csv", nrows = 160)
library(dplyr)

```

`@sample_code`
```{r}
# Vaata üle tunnusenimed ja andmestiku dimensioonid
names(B)
dim(B)

# Ülesanne 1: Alamandmestiku valik
B1 <- _____ %>% _______(_______)



# Ülesanne 2: Täienda koodi
library(____________)
tabel <- B1 %>% _____________()  %>%  _____________(___________)  %>%  ______________(.funs = c("mean", "sd", "min", "max"))
arrange(tabel, variable)
 

```

`@solution`
```{r}
# Vaata üle tunnusenimed ja andmestiku dimensioonid
names(B)
dim(B)

# Ülesanne 1: Alamandmestiku valik
B1 <-  B %>% select(starts_with("test"))



# Ülesanne 2: Täienda koodi
library(reshape2)
tabel <- B1 %>% melt()  %>%  group_by(variable)  %>%  summarise_all(.funs = c("mean", "sd", "min", "max"))
arrange(tabel, variable)
```

`@sct`
```{r}

#1
test_function("select",
              args = c(".data"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta esimeses ülesandes funktsiooni `select()`.",
              args_not_specified_msg = paste("Funktsioonile `select()` pead aheldamisega saatma esimeseks argumendiks andmestiku `B`." ),
              incorrect_msg = paste("Funktsioonis `select()` on argumendi `.data` väärtus vale. Selleks andmestikuks peab olema `B`."))
       


test_function("starts_with",
              args = c("match"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta esimeses ülesandes funktsiooni `starts_with()`.",
              args_not_specified_msg = paste("Funktsioonis `starts_with()` pead määrama  argumendiks sobiva tunnusenime alguse jutumärkides. Praegu on selleks `'test'`." ),
              incorrect_msg = paste("Funktsioonile `starts_with()`    antud tunnusenime algus on vale, kirjuta see samal kujul nagu ülesande tekstis."))
       

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
              incorrect_msg =  "Käsu `library()` argumendiks anna paketi nimi `reshape2`")



test_function("melt",
              args = c("data"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta teises ülesandes funktsiooni `melt()`.",
              args_not_specified_msg = paste("Funktsioonile `melt()` peab aheldamise kaudu argumendiks minema andmestik `B1`. Teisi argumente antud funktsioonis pole vaja täpsustada." ),
              incorrect_msg = paste("Funktsioonile `melt()` saadetakse argumendiks vale andmestik. KAsuta andmestikku `B1`."))
       




test_function("group_by",
              args = c(".data"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta teises ülesandes funktsiooni `group_by()`.",
              args_not_specified_msg = paste("Funktsiooni `group_by()` esimeseks argumendiks peab aheldamine saatma pikas formaadis andmestiku. Grupeerivaks tunnuseks peab minema veerg, kus on kirjas testide nimed. " ),
              incorrect_msg = paste("Funktsiooni `group_by()`  rakendatakse valele andmestikule. "))
       

 



test_function("summarise_all",
              args = c(".tbl", ".funs"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta teises ülesandes funktsiooni `summarise_all()`.",
              args_not_specified_msg = paste("Funktsiooni `summarise_all()`", 
                                c(" esimeseks argumendiks peab sattuma grupeeritud andmestik.", "teiseks arumendiks peab olema vektor funktsioonide nimedega, mida tahad kasutada.") ),
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

---

## Mitmele tunnusele teisenduse määramine

```yaml
type: NormalExercise
key: edea2d2fcc
lang: r
xp: 100
skills: 1
```

Nagu öeldud on lisapaketis **dplyr** on olemas funktsioonide `mutate()` ja `summarise()` eriversioonid sufiksitega `_at`, `_if` ja `_all` juhuks kui fuktsioone on vaja rakendada mingite tunnuste valikule nime järgi, loogilise tingimuse järgi või kõigile tunnustele, mis ei esine grupeeriva tunnusena. 



Töölaual on olemas andmestikud `mass` (tuttav praktikumist) ja `antropo`. Andmestikus  `antropo` on mõned antropomeetrilised mõõtmised, mõõdetud on  pikkus õlani (mm), kerepikkus istudes (mm),  põlve kõrgus, istudes (mm),  talje ümbermõõt (mm),  õlgade ümbermõõt (mm),  randmeümbermõõt (mm) ja  kaal (kg*10), lisaks on teada uuritava sugu.

Aktiveeritud on pakett **dplyr**.

`@instructions`
- **Ülesanne 1:**  Teisenda andmestikus `mass` kõik tunnused, mis on tüüpi `factor` tavaliseks tekstiks ehk tüüpi `character`. Selleks täienda antud koodi sobivalt. Teisendatud tunnustega andmestik nimeta `mass_char`. Koodi kirjapanekul kasuta aheldamisoperaatorit `%>%`.
- **Ülesanne 2:** Teisenda andmestikus `antropo` kõik mõõtmised, mis on millimeetrites sentimeetriteks ja tunnus, mille mõõtühik on 10*kg, kilogrammidesse (st teisendada tuleks kõik tunnused peale soo tunnuse). Defineeri selleks uus funktsioon, mida saad edasi kasutada sobiva sufiksiga `mutate_`-käsus. Teisenduse tegemiseks täienda antud koodi. Täiendamisel kasuta aheldamisoperaatorit `%>%`, muudetud andmestik omista muutujale `antropo_cm_kg`.

`@hint`
- Selleks, et kontrollida, kas tunnus on faktortüüpi, saab kasutada funktsiooni `is.factor()`.
- Selleks, et tunnuse tüüpi määrata tavaliseks tekstitüübiks, kasuta funktsiooni `as.character()`.
- Uus funktsioon peaks läbi viima jagamise arvuga 10, `function(x) x/10`.

`@pre_exercise_code`
```{r}
antropo <- read.table("http://kodu.ut.ee/~annes/R/antropo.txt",header=T,sep="\t")
mass<- read.table("http://kodu.ut.ee/~annes/Rkursus/mass.txt",header=T,sep="\t")

library(dplyr)

```

`@sample_code`
```{r}
# Tutvu andmestikega
str(mass)
str(antropo)

# Ülesanne 1: teisenda faktorid tavaliseks tekstiks
_________ <-  __________ %>% mutate______(.predicate = __________, .funs = __________)
str(mass_char)

# Ülesanne 2: teisenda ühikud
uus_funktsioon <- function(_____) ____________
antropo_cm_kg <- _______  ____  mutate______(.vars = vars(-SEX), .funs = uus_funktsioon)
head(antropo_cm_kg)
```

`@solution`
```{r}
# Tutvu andmestikega
str(mass)
str(antropo)

# Ülesanne 1: teisenda faktorid tavaliseks tekstiks
mass_char <-  mass %>% mutate_if(.predicate = is.factor, .funs = as.character)
str(mass_char)

# Ülesanne 2: teisenda ühikud
uus_funktsioon <- function(x) x/10
antropo_cm_kg <- antropo %>% mutate_at(.vars = vars(-SEX), .funs = uus_funktsioon)
head(antropo_cm_kg)

```

`@sct`
```{r}
# 1
test_function("mutate_if",
              args = c(".tbl", ".predicate", ".funs"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta esimeses ülesandes funktsiooni `mutate_if()`.",
              args_not_specified_msg = paste("Funktsiooni `mutate_if()`", 
                                c(" esimeseks argumendiks peab sattuma andmestik `mass`, st see saadetakse aheldamisega funktsiooni esimeseks argumendiks.", 
                                " peab määrama loogilise kontrolli (`.predicate`), millele vastavatele veergudele hakatakse teisendust tegema. Kontrollima peab, kas veerus on `factor`-tüüpi tunnus.", 
                                " peab määrama teisenduse (`.funs`), mida valitud veergudele rakendada, see peaks veeru tüübiks määrama `character`") ),
              incorrect_msg = paste("Funktsiooni `mutate_if()`  ",  c(" rakendatakse  valele andmestikule.", 
              " on antud vale tingimus (`.predicate`), millele veerud peavad vastama.", 
              " on määratud vale teisendus (`.funs`).")   ))
 

 
 
  
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
              args = c(".tbl", ".vars", ".funs"), index = 1,
              eval = c(TRUE, TRUE,  TRUE),
              eq_condition = "equivalent",
              not_called_msg = "Kasuta teises ülesandes funktsiooni `mutate_at()`.",
              args_not_specified_msg = c("Funktsiooni `mutate_at()` esimeseks argumendiks peab sattuma andmestik `antropo`, st see saadetakse aheldamisega funktsiooni esimeseks argumendiks.", 
                                " Ära kustuta funktsiooni `mutate_at()` etteantud argumentide väärtusi.", 
                                " Ära kustuta funktsiooni `mutate_at()` etteantud argumentide väärtusi.") ,
              incorrect_msg = c("Funktsiooni `mutate_at()`   rakendatakse  valele andmestikule.", 
              "Ära muuda funktsiooni `mutate_at()` etteantud argumentide väärtusi.", 
              "Ära muuda funktsiooni `mutate_at()` etteantud argumentide väärtusi või kirjapilti.")   )

			  
			  
			  
			  

test_pipe(num = 2, absent_msg = "Kasuta aheldamisoperaatorit `%>%`.", insuf_msg = "Kasuta aheldamisoperaatorit `%>%` vähemalt 2 korda.") 
 
 
 
test_data_frame("antropo_cm_kg",
                columns = NULL,
                eq_condition = "equivalent",
                undefined_msg = "Andmestikku `antropo_cm_kg` pole tekitatud!",
                undefined_cols_msg = paste("Andmestikus `antropo_cm_kg` on mõni veerg puudu! "),
                incorrect_msg = paste("Andmestikus `antropo_cm_kg` on mõni veerg valede väärtustega!"))
                
                
 
                
                
                
                
                
                
success_msg("Ülesanne tehtud. Tubli!")               
       





```

---

## Andmestike ühendamine

```yaml
type: NormalExercise
key: cfbdcdcc57
lang: r
xp: 100
skills: 1
```

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

`@instructions`
- **Ülesanne 1:** Teisenda esmalt mõlemas andmestikus faktortunnused tavaliseks tekstiks, selleks täienda etteantud koodi. Teisendatud andmestikud omista muutujatele `A1` ja `B1`. Edasi kasuta neid andmestikke.

- **Ülesanne 2:** Ühenda andmestikud id-koodi tunnuse põhjal, nimeta ühendatud andmestik nimega `AB1`. Tulemuseks olevas andmestikus peaks olema ainult need uuritavad, kelle kohta on olemas info mõlemas andmestikus, kuid veergudest ainult need, mis on olemas andmestikus `B`. Ühendamise läbiviimiseks vali üks `_join()` funktsioon ülaltoodud nimekirjast. Koodi kirjapanekul kasuta `%>%` operaatorit.

- **Ülesanne 3:** Ühenda andmestikud id-koodi tunnuse põhjal, nimeta ühendatud andmestik nimega `AB2`. Tulemuseks olevas andmestikus peaks olema ainult need uuritavad, kes on andmestikus `A` ja kellel on vaste andmestikus `B` ning  kõik veerud mõlemast andmestikust. Ühendamise läbiviimiseks vali üks `_join()` funktsioon ülaltoodud nimekirjast. Koodi kirjapanekul kasuta `%>%` operaatorit.

`@hint`
- Esimeses ülesandes kasuta faktortunnuste sõnedeks teisendamisel argumente `.predicate = is.factor` ja `.funs = as.character`.

`@pre_exercise_code`
```{r}
A <- read.csv2("http://kodu.ut.ee/~annes/R/A.csv", nrows = 45)
B <- read.csv2("http://kodu.ut.ee/~annes/R/B.csv", nrows = 160)
B <- B[, c("id", "grupp", sort(names(B)[-(1:2)]))]

library(dplyr)

```

`@sample_code`
```{r}
# Vaata anmdestikud üle
str(A)
str(B)


# Ülesanne 1:  Teisenda tunnuse tüüp
A1 <- A %>% mutate_if(_____, _____)
B1 <- B %>% mutate_if(_____, _____)


# Ülesanne 2: ühenda andmestikud
AB1 <- ___ %>% _______(_____, _____)
head(AB1)

# Ülesanne 3: ühenda andmestikud
AB2 <- ___ %>% _______(_____, _____)
head(AB2)

```

`@solution`
```{r}
# Vaata andmestikud üle
str(A)
str(B)


# Ülesanne 1:  Teisenda tunnuse tüüp
A1 <- A %>% mutate_if(is.factor, as.character)
B1 <- B %>% mutate_if(is.factor, as.character)


# Ülesanne 2: ühenda andmestikud
AB1 <- B1 %>% semi_join(A1, by = "id")
head(AB1)

# Ülesanne 3: ühenda andmestikud
AB2 <- A1 %>% inner_join(B1, by = "id")
head(AB2)

```

`@sct`
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
             not_called_msg = "Pead kolmandas ülesandes  kasutama  funktsiooni `inner_join()`.",
             args_not_specified_msg = paste("Käsus `inner_join()`  ", c("saadetakse esimene liidetav andmestik läbi aheldamisoperaatori", " tuleb märkida teine liidetav andmestik", "tuleb määrata võtmetunnus `by`", "." )),
             incorrect_msg = paste("Muuda `inner_join()` käsus ", c("läbi aheldamise saadetav andmestik.", " teine liidetav andmestik. ", "argumendi `by` väärtus.")) )
,
test_function(name = "inner_join",
              args = c("by"),
              index = 1,
             eval = TRUE,
             eq_condition = "equivalent",
             not_called_msg = "Pead kolmandas ülesandes  kasutama  funktsiooni `inner_join()`",
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
