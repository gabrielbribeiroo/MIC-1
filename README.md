# MIC-1

![Alt text](/80085.jpg?raw=true)

## Interpreter koda

Ovaj kod sluzi za prevodjenje mikroasemblerskih instrukcija u binarno-enkodiranu instrukciju koju ogledni procesor moze koristiti. 
Primjer koristenja:

```cpp
    init(); // Mora se pozvati!!!

    std::vector<std::pair<int, std::string>> rets =
    {
        { 1, "ac := ac + (1);" },
        { 2, "ac := ac + (1);" },
        { 3, "ac := ac + (1);" },
        { 4, "rd; goto 82" },
        { 80, "mar := ir; rd;" },
        { 81, "b := (0); rd;" },
        { 82, "a := mbr;" },
        { 83, "alu := a; if z then goto 88;" },
        { 84, "alu := a ^ (1); if z then goto 86" },
        { 85, "b := b + ac;" },
        { 86, "ac := ac + ac;" },
        { 87, "a := rshift ( a ); goto 83;" },
        { 88, "ac := b;" }
    };

    for (auto &instr : rets)
        std::cout << std::setw(5) << instr.first << " => \"" <<  parse(instr.second) << "\",\n";
```

Kod iznad implementira MULD m instrukciju, pri cemu je ac := 3 na pocetku. Output ovog primjera je:

```cpp
    1 => "00000000000100010000000100000000",
    2 => "00000000000100010000000100000000",
    3 => "00000000000100010000000100000000",
    4 => "01110000010000000000000001010010",
   80 => "00010000110000000011001100000000",
   81 => "00010000010110110000000000000000",
   82 => "10010000000110100000000000000000",
   83 => "01010000000000000000101001011000",
   84 => "01001000000000000000101001010110",
   85 => "00000000000110110001101100000000",
   86 => "00000000000100010001000100000000",
   87 => "01110010000110100000101001010011",
   88 => "00010000000100010000101100000000"
```
koji se stavlja u ROM komponentu oglednog procesora. Ovaj skup instrukcija je uspjesno testiran na VHDL test benchu oglednog procesora.

This project is licensed under the [MIT License](LICENSE).

Limitacije:
* Konstante 1, -1 i 0 se obiljezavaju sa (+1), (0), (-1) respektivno
* Umjesto band koristiti ^ operator

Kod je testiran sa 10ak drugih instrukcija, no autor je svjestan velike mogucnosti pojavljivanja bugova. Takodje je potrebno imati u vidu da je kod
pisan u malom vremenskom periodu.
