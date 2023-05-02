# Frequency modulation synthesis

## モジュール
帰省した時にYM2612とYM3812をガチャガチャであてた。
http://ym2203.com/

## YM2612
OPN系と言われるチップらしい。

- https://ja.wikipedia.org/wiki/YM2612
- https://en.wikipedia.org/wiki/Yamaha_YM2612
- https://github.com/skywodd/avr_vgm_player/tree/master/ym2612_test
- http://www.smspower.org/maxim/Documents/YM2612
- http://valsound.fc2web.com/fm_lib.html

## チップをみてみる
```
name:         YM2612
function:     FM sound generator
package:      DIP,24
manufacturer: Yamaha
added-by:     Andre Delavy
comment:      Genesis Megadrive FM chip

    +--()--+
GND | 1  24| 0M(CLK)
 D0 | 2  23| Vcc
 D1 | 3  22| A.Vcc
 D2 | 4  21| MOL
 D3 | 5  20| MOR
 D4 | 6  19| A.GND
 D5 | 7  18| A1
 D6 | 8  17| A0
 D7 | 9  16| /RD
  ? |10  15| /WR
/IC |11  14| /CS
GND |12  13| /IRQ
    +------+

This information was added by a third party and may be incorrect.

This pinout came from the Chipdir:
http://www.chipdir.org/
```
クロックとして、8MHzとか7.670MHzのクロックがいるらしい。
8ビットの入力がいるんだな。
5Vがいるのか、昇圧DCDCがいるな。

## サンプルをみてみる
https://github.com/skywodd/avr_vgm_player/blob/master/ym2612_test/ym2612_test.c

データ線の他に、IC,CS,WR,RD,A0,A1が繋ってる。

初期化でA0とA1だけLowにして後はHighにしてる。

ICを一旦Lowにして、Highにする事でチップのリセットがおこなわれるようだ。

A1はチャンネル1-3か4-6を選択してるようだ。

CSをLowにして、データを準備して、
WRをLowにしてHighにして書き込み完了で、
CSをHighに戻す。
これずっとCSをLowにしとくんじゃだめかな。

A0をLowにしておくとアドレスを選択のために書いて、
Highにすると、データを書き込むようだ。

タイミングチャートがあるデータシートがないからあってるか自信ないが。

## レジスタをみてみる

## FM音源をみてみる
