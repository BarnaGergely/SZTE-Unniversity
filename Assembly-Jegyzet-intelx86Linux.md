# Assembly 2. házifeldat részletes megoldás

## Ciklusok működése

Assembly-ben nincsenek ciklusok. Fut a program vegrehajtasa fentrol lefele sorrol sorra,
mi csak a ugralni tudunk benne feljebb es lejebb cimkekre.

## Ugrás működése

`jmp <memoriacim>` - a vezerles a <memoriacim>-re ugrik es onnan folytatodik

Ne feledd a cimkek sima memoriacimekkent viselkednek, igy lehet rajuk ugrani.

### Vegtelen ciklus:

```assembly
kezdet:
// program sorai
jmp kezdet
```

### feltételes ugrás

Vannak feteteles ugro utasitasok is, amik csak akkor ugranak, ha egy feltetel teljesul. _(ez nem igaz, a háttérben nem igy mukodik, de igy a legerthetobb és a használata a mi feladatainkban ilyen)_

```assembly
cmp edx, 42  // edx es a 42 osszehasonlitasa
jz kezdet    // ha az elozo cmp utasitasban megadott adatok egyenlok, a kezdet cimkere ugrik
```

#### Osszehasonlito utasitasok:

| Relacio | Eloojeles | Elojeltelen |
| ------- | --------- | ----------- |
| ==      | je, jz    | je, jz      |
| !=      | jne, jnz  | jne, jnz    |
| <       | jl, jnge  | jb, jnae    |
| <=      | jle, jng  | jbe, jna    |
| >       | jg, jnle  | ja, jnbe    |
| >=      | jge, jnl  | jae, jnb    |

## indirekt címzés

Kapcsoszárójelekkel tudjuk elérni egy memória címen tárolt adatok értékét.

- `[eax]` - az eax regiszterben lévő számot memória címként használva megpróbál betölteni a memóriából egy adatot. Ha memória cím volt benne vagy szerencséd volt és pont létező memóriára mutat a benne lévő szám, megkapot az általa mutatott értéket.

- `[CIMKE]` - A CIMKE címkén (ami ugye egy memória címként működik) lévő adatot kéri be a memóriából

A kapcsoszárójelekben lévő memóriacímhez tudunk hozzá adni. Ez annyi bájttal elcsúsztatja a memóriacímet, amennyit hozzá adtunk. Az integer tipusú változóink 4 bájtosak, szóval ha egy tömb következő eleme érdekel 4-et kell hozzá adnod a memória címhez.

- `[TOMB+0*4]` - TOMB címkén lévő adatsorozat _(ismertedd nevén tömb)_ 1. eleme
- `[TOMB+1*4]` - TOMB címkén lévő tömb 2. eleme
- `[TOMB+2*4]` - TOMB címkén lévő tömb 3. eleme

Ha megfigyeled ez olyan, mint egy C-s tömb: _NULLÁTÓL indexelünk!!!_

Egy másik regiszterrel akár dinamikusan is indexelhetünk egy tömböt

- `[SZAMOK+eax*4]` - TOMB címkén lévő "tomb" eax-edik eleme

És persze az eltolás is működik regiszterekből bejövő címekkel is.

- `[ebx+0*4]` - ebx-ben lévő memóriacímen lévő tömb 1. eleme
- `[ebx+eck*4]` - ebx-ben lévő memóriacímen lévő tömb eck-edik eleme

mov, add, cmp, stb. utasításokban **nem használhatunk mindkét paramétérben indirekt címzést!!!!!!!**

Ez hibás (el se olvasd!): `mov [TOMB + 2*4], [eax + edx*4] // HIBÁS`

```assembly
mov ecx, [eax + edx*4]
mov [TOMB + 2*4], ecx // HELYES
```