#difftime
Function: _double_ **difftime** _(time_t end, time_t begin)_
保证不会发生溢出，但是受制于double的形式可能不会很精确。
某些系统会禁止time_t 直接相减，那就只能用difftime了。

