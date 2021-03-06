# Algorithm functions

## swap(left, right) : Swap two cells #0 and #1
    /**
     *  INPUT:  left & right
     *  CODES:
     *          var temp = 0;
     *          temp  = right;
     *          right = left;
     *          left  = temp;
     */
    
    >
        >[-]<       // clear cell #2
        [->+<]      // move cell #1 forward
    <
    [->+<]          // move cell #0 forward
    >>[-<<+>>]<<    // move cell #2 back to #0

## max(a, b) : Compare cell #0 and #1, save the maximum in #2
    /**
     *  INPUT:  a & b
     *  CODES:
     *          var result = a;
     *          var temp = b;
     *          var _a = a;
     *          var _b = b;
     *          if (_a SMALLER_THAN _b) {
     *              result = temp;
     *          }
     */
    
    /**
     *  1st:  prepare cells { a b 0 0 0 0 0 } to { a b a b a b 0 }
     */
    >>[-]>[-]>[-]>[-]>[-]<<<<<< // clear cells #2~6
    [->>+>+>+<<<<]              // copy cell #0 to #2 #3 #4
    >>>[-<<<+>>>]<<<            // move cell #3 back to #0
    >[->>+>>+>+<<<<<]<          // copy cell #1 to #3 #5 #6
    >>>>>>[-<<<<<+>>>>>]<<<<<<  // move cell #6 back to #1
    
    >>>>                        // check cells: { a b a b(a b)0 }
        /**
         *  2nd:  compare(#4 AND #5);
         */
        [->                     // decrease cell #4
                [               // if cell #5 not 0
                    -[->+<]     //     decrease and move to #6
                ]
                >[-<+>]<        // move cell #6 back to #5
        <]
        /**
         *  3rd:  if (#4 SMALLER_THAN #5) {
         *            #2 = #3;
         *        }
         */
        >[                      // if cell #5 not equls 0
            <<<[-]>[-<+>]       //     move cell #3 to #2
            >>[-]               //     clear cell #5
        ]<
        <[-]>                   // clear cell #3
    <<<<

## min(a, b) : Compare cell #0 and #1, save the minimum in #2
    /**
     *  INPUT:  a & b
     *  CODES:
     *          var result = b;
     *          var temp = a;
     *          var _a = a;
     *          var _b = b;
     *          if (_a SMALLER_THAN _b) {
     *              result = temp;
     *          }
     */
    
    /**
     *  1st:  prepare cells { a b 0 0 0 0 0 } to { a b b a a b 0 };
     */
    >>[-]>[-]>[-]>[-]>[-]<<<<<< // clear cells #2~6
    [->>+>+>+<<<<]              // copy cell #0 to #2 #3 #4
    >>[-<<+>>]<<                // move cell #2 back to #0
    >[->+>>>+>+<<<<<]<          // copy cell #1 to #2 #5 #6
    >>>>>>[-<<<<<+>>>>>]<<<<<<  // move cell #6 back to #1
    
    >>>>                        // check cells: { a b b a(a b)0 }
        /**
         *  2nd:  compare(#4 AND #5);
         */
        [->                     // decrease cell #4
                [               // if cell #5 not 0
                    -[->+<]     //     decrease and move to #6
                ]
                >[-<+>]<        // move cell #6 back to #5
        <]
        /**
         *  3rd:  if (#4 SMALLER_THAN #5) {
         *            #2 = #3;
         *        }
         */
        >[                      // if cell #5 not equls 0
            <<<[-]>[-<+>]       //     move cell #3 to #2
            >>[-]               //     clear cell #5
        ]<
        <[-]>                   // clear cell #3
    <<<<

## sort(array) : Sort the array starts from cell #1, head(#0) and tail(#n+1) are 0
    /**
     *  INPUT:  array
     *  CODES:
     *          int cnt = count(array);
     *          int flag = cnt;
     *          for (int k; k = flag;) {
     *              flag = 0;
     *              for (int j = 1; j SMALLER_THAN k; INC j) {
     *                  if (item(j_1) GREATER_THAN item(j)) {
     *                      swap(item(j_1) AND item(j));
     *                      flag = j;
     *                  }
     *              }
     *          }
     */
    
    [-]     // make sure the head is 0
    >[>]    // go to the tail
    
    /**
     *
     *  1st: prepare var_table: { r0 r1 r2 j k flag cnt t }
     *
     */
    >[-]>[-]>[-]>[-]
    >[-]>[-]>[-]>[-]
    <<<< <<<<
    
    /**
     *
     *  get count of the array item(s) and save it in cnt(#6)
     *
     */
    <[                          // { (last)|0 0 0 j=0 k=0 flag=0 cnt=? 0 }
        [->>>> >>>>+<<<< <<<<]  // move item left to right
        >>>> >>> [-<+>]<+       // move counter backward and increase it
        <<<< <<<                // var_table steps backward
    ]>                          // { head|(0) 0 0 j=0 k=0 flag=0 cnt=C 0|first }
    
    /**
     *
     *  move var_table back to tail
     *
     */
    >>>> >>>>[                  // { head|0 0 0 j=0 k=0 flag=0 cnt=C 0|(first) }
        [-<<<< <<<<+>>>> >>>>]  // move the item right to left
        <<[->+<]>>              // move counter forward
        >                       // var_table steps forward
    ]<<<< <<<<                  // { last|(0) 0 0 j=0 k=0 flag=0 cnt=C 0 }
    
    /**
     *
     *  2nd: Bubble Sort
     *
     *      k = flag = count;   // we can keep cnt=C here if needed
     */
    >>>> >>[-<<+>>]<<           // { last| 0 0 c j=0 (k=C) flag=0 cnt=0 0 }
    /**
     *      while (k) {
     */
    [
        [-<+<+>>]               // { a b | 0 0 c j=c (k=0) flag=0 cnt=C 0 }
        <<[->>+<<]              // { a b | 0 0 (0) j=c k=c flag=0 cnt=C 0 }
        /**
         *      for (j = k_1; j; __j) {
         */
        >-[<<<
            <<[->>+>+<<<]>>     // { 0 b |(a) a 0 j=? k=c  flag=? cnt=C 0 }
            >[-<<<+>>>]<        // { a b |(a) 0 0 j=? k=c  flag=? cnt=C 0 }
            <[->                // decrease(b);
                [-[->>+<<]]     // r0 = a minus b until 0
                >>[-<<+>>]<<    // use r2 to store the temp value of r0
                >+<             // increase(r1);
            <]>                 // { a 0|(a~b) b 0 j=? k=c flag=? cnt=C 0 }
            >[-<<+>>]<          // { a b|(a~b) 0 0 j=? k=c flag=? cnt=C 0 }
            /**
             *      if (a GREATER_THAN b) {
             */
            [[-]
                /**
                 *      swap(a b);
                 */
                <[->+<]         // { a(0)| b  0 0 j=? k=c flag=? cnt=C 0 }
                <[->+<]         // { (0)a| b  0 0 j=? k=c flag=? cnt=C 0 }
                >>[-<<+>>]      // {  b a|(0) 0 0 j=? k=c flag=? cnt=C 0 }
                
                /**
                 *      flag = k~j;
                 */
                >>>>
                >[-]<               // { b a|0 0 0 j=? (k=c) flag=0 cnt=C 0 }
                [->+>>+<<<]         // { b a|0 0 0 j=? (0)        k     C k }
                >>>[-<<<+>>>]<<<    // { b a|0 0 0 j=? (k)        k     C 0 }
                <[->>->>+<<<<]>     // { b a|0 0 0 0   (k)      k~j     C j }
                >>>[-<<<<+>>>>]<<<  // { b a|0 0 0 j   (k)      k~j     C 0 }
                <<<<
            ]                       // { b a|(0) 0 0 j  k       k~j     C 0 }
            /**
             *      } // if (a b) END
             */
            
            /**
             *
             *  move the var_table step backward
             *
             */
            <[->>>> >>>>+<<<< <<<<]>
            >>>-                // decrease(j);
            [-<+>]>             // backward(j);
            [-<+>]>             // backward(k);
            [-<+>]>             // backward(flag);
            [-<+>]<<<<          // backward(cnt);
        ]
        /**
         *      }  //  for(;j;) END
         *      
         *      k = flag;
         *      flag = 0;
         */
        >[-]>[-<+>]             // { 0 0 0 j k=flag (flag=0) cnt=C 0 }
        
        /**
         *
         *  move the var_table back to tail
         *
         */
        >>>[                    // { head|0 0 0 j=0 k=c flag=0 cnt=C 0|(first) }
            [-<<<< <<<<+>>>> >>>>]  // move item right to left
            <<[->+<]                // forward(cnt);
            <<[->+<]                // forward(k);
            >>>> >                  // var_table steps forward
        ]<<<<                   // { last|0 0 0 (j=0) k=c flag=0 cnt=C 0 }
    ]
    /**
     *      }  //  while(k) END
     */
    <<<<                        // { last|(0) 0 0 j=0 k=0 flag=0 cnt=C 0 }
    
    <[<]    // pointer go back to the head
