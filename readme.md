# Charakterystyka danych 

*Wyposażeni w odpowiednie narzędzia schodzimy pod ziemię, na odpowiednią głębokość, 
w celu znalezienia drogocennych materiałów występujących w grupach o różnym rozmiarze.*

W strumieniu pojawiają się zdarzenia zgodne ze schematem `MinecraftEvent`.

```
create json schema MinecraftEvent(ore string, depth int, amount int, ets string, its string);
```

Każde zdarzenie związane z jest z faktem wydobycia określonego surowca o określonym 
rozmiarze z określonej głębokości. 

Dane uzupełnione są o dwie etykiety czasowe zaokrąglone do jednej sekundy. 
* Pierwsza (`ets`) związana jest z momentem wydobycia surowca. 
  Etykieta ta może się losowo spóźniać w stosunku do czasu systemowego maksymalnie do 60 sekund.
* Druga (`its`) związana jest z momentem rejestracji zdarzenia w systemie.

# Opis atrybutów

Atrybuty w każdym zdarzeniu zgodnym ze schematem `MinecraftEvent` mają następujące znaczenie:

* `ore` - rodzaj surowca należący do zbioru (`coal`, `iron`, `gold`, `diamond`, `emerald`)
* `depth` - głębokość, z której wydobyto surowiec, zakres od 1 do 35
* `amount` - wielkość wydobycia, zakres od 1 do 9
* `ets` - czas wydobycia surowca
* `its` - czas rejestracji zdarzenia w systemie.

# Zadania
Opracuj rozwiązania poniższych zadań. 
* Opieraj się strumieniu zdarzeń zgodnych ze schematem `MinecraftEvent`
* W każdym rozwiązaniu możesz skorzystać z jednego lub kilku poleceń EPL.
* Ostatnie polecenie, będące ostatecznym rozwiązaniem zadania, musi 
  * być poleceniem `select` 
  * posiadającym etykietę `answer`, przykładowo:

```aidl
  @name('answer') SELECT ore, depth, amount, ets, its
  FROM MinecraftEvent#ext_timed(java.sql.Timestamp.valueOf(its).getTime(), 3 sec)
```

## Zadanie 1
Utrzymuj informację o sumie wydobytego surowca dla każdego jego typu 
zarejestrowanych w systemie w ciągu ostatniej minuty. 

Wyniki powinny zawierać, następujące kolumny:
- `ore` - typ surowca
- `sumAmount` - suma wydobytego surowca zarejestrowana w ciągu ostatniej minuty.

## Zadanie 2
Wykrywaj przypadki, w których napotkano diamenty o liczności większej niż 6 
na głębokości większej niż 12.

Wyniki powinny zawierać, następujące kolumny:
- `depth` - głębokość, na jakiej znaleziono diamenty
- `amount` - wielkość wydobytego surowca
- `ets` - czas wydobycia surowca
- `its` - czas rejestracji.

## Zadanie 3
Znajdź przypadki, w których wydobyto surowiec o wielkości minimum 1.5 razy większej niż 
średnia wielkość wydobycia tego samego rodzaju surowca w ciągu ostatniej minuty.

Wyniki powinny zawierać, następujące kolumny:
- `ore` - rodzaj wydobytego surowca
- `depth` - głębokość
- `amount` - wielkość wydobycia 
- `ets` - czas natrafienia 
- `its` - czas rejestracji.

## Zadanie 4
Dla dwóch poziomów głębokości: (1) małej — mniej niż 10 i (2) dużej — większej niż 20, 
utrzymywane są statystyki dotyczące wydobycia zarejestrowanego w ciągu ostatniej minuty 
dla każdego z typów surowca.

Utrzymuj porównanie tych samych surowców dla obu poziomów głębokości. 

Wyniki powinny zawierać, następujące kolumny:
- `ore` - rodzaj surowca
- `sumAmountHeaven` - wielkość wydobytego surowca na małej głębokości 
- `sumAmountHell` - wielkość wydobytego surowca na dużej głębokości.

## Zadanie 5
Znajduj serie wydobycia dowolnego rodzaju surowca zakończone wydobyciem diamentów 
o wielkości większej niż 5. Maksymalny czas serii nie może przekroczyć 30 sekund.

Zadbaj o to, aby znalezione serie się nie pokrywały.

Wyniki powinny zawierać, następujące kolumny:
- `ore` - rodzaj surowca zdarzenia początkowego
- `depth` - głębokość wydobycia dla zdarzenia początkowego
- `amount` - wielkość wydobytego surowca zdarzenia początkowego
- `startEts` - czas wydobycia zdarzenia początkowego
- `endEts` - czas wydobycia diamentów kończącego serię

## Zadanie 6
Znajduj trójki zdarzeń wydobycia tego samego surowca, każdorazowo o wielkości 
powyżej 5, przy czym każde kolejne wydobycie będzie większe od poprzedniego. 
Odrzuć takie przypadki, w trakcie których wydobyto surowiec innego rodzaju.

Wyniki powinny zawierać następujące kolumny:
- `ore` - rodzaj surowca w wykrytej serii
- `amount1` - wielkość wydobycia w pierwszym zdarzeniu
- `amount2` - wielkość wydobycia w drugim zdarzeniu
- `amount3` - wielkość wydobycia w trzecim zdarzeniu

## Zadanie 7
Trzy kolejne wydobycia trzech różnych surowców są określane przez graczy *Big3*. 
Znajduj przypadki wystąpienia dwóch następujących po sobie *Big3*. 
Każde wydobycie może należeć tylko i wyłącznie do jednego *Big3*.

Wyniki powinny zawierać, następujące kolumny:
- `startIts` - moment rejestracji pierwszego zdarzenia z pierwszego *Big3*
- `endIts` - moment rejestracji ostatniego zdarzenia z drugiego *Big3*.
- `sumAmount` - wielkość wydobytego surowca ze wszystkich sześciu zdarzeń.
