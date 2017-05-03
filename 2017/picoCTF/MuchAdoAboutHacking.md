# Much Ado About Hacking

## The chall

This challenge was about esoterism, particularly the [Shakespeare Language](https://esolangs.org/wiki/Shakespeare).  
We were provided a program written in Shakespeare, asking how to produce the output `tu1|\h+&g\OP7@% :BH7M6m3g=`.  

The given program was MuchAdoAboutHacking.spl, which after examining gave the following pseudo-code:

    DonJohn = 0
    DonPedro = 0
    
    while Benedick.last != 32:
      Beatrice++
      Benedick.append(input_char)
    
    Beatrice--
    Benedick.pop
    
    while Beatrice > 0:
      Benedick.last -= 32
      Beatrice--
      DonJohn = Benedick.last
      Benedick.last += DonPedro
      Benedick.last %= 96
      DonPedro = DonJohn
      Benedick.last += 32
      print Benedick.last
      Benedick.pop
 
Phew. It's a bit rough, mainly because I kept it as it is in Shakespeare, not an optimized version.
So basically, we read the input, we stop when we see the character 32 (a space) and then we take the stack from top
to bottom, making the operation `(x-32+last)%96+32` and we print the result. No big deal, it should be solvable in Shakespeare.
 
## The solution
 
We run into a few issues here:
 * the input is read from the last character to the first one, and we don't have a `reverse` function in Shakespeare.
 * the characters are output from the last character to the first one, same problem.
 * there is a modulo. Not a real issue but still scary at first.
 * there are spaces in the string we are asked to find.
 
To address the first issue, we shall use another list in order to reverse the result twice, so that we can read the answer
from left to right, say `Romeo` for instance. Hence, we need an other character to handle the index, say `Juliet` for instance.

For the second issue, as we have cleared `Benedick` to reverse its content, we only need an index, say `Hamlet` for instance.
 
In order to defeat the modulo situation, we just need not to have negative numbers, no big deal here: all we need to do is add
`Cleopatra` before doing the modulo operation.

About spaces, we had to change the control character to something that was not used, I chose the tabulation as it was easy to do.

The operations we will do will thus be the following:
    DonJohn = 0
    DonPedro = 0
    
    while Benedick.last != 9:
      Beatrice++
      Benedick.append(input_char)
    
    Beatrice--
    Benedick.pop
    
    Juliet = Beatrice
    Hamlet = Beatrice
    
    while Juliet > 0:
      Romeo = Benedick.last
      Benedick.pop
      Juliet--
    
    while Beatrice > 0:
      Romeo.last -= 32
      Beatrice--
      Romeo.last -= DonPedro
      DonJohn = Romeo.last
      if Romeo.last < 0:
        Romeo.last += 96
      Romeo.last %= 96
      DonPedro = DonJohn
      Romeo.last += 32
      Benedick.append(Romeo.last)
    
    while Hamlet > 0
      print Benedick.last
      Benedick.pop
      Hamlet--
      
That's quite a few more lines, but we managed to do it, you can find the Shakespeare version in the file MuchAdoAboutCracking.spl
 
