nl
====

### 语法

```
nl [options] files
```

为文件里每一行加上行号。


***

### 参数

* `-b | --body-numbering=STYLE` 设定内容的编号方式，默认方式为`t`(只有非空白行才有编号)。

* `-d | --section-delimiter=CC` 设定逻辑分隔符CC

* `-f | --footer-number=SYTLE` 设定页脚的编号方式，默认方式为`n`

* `-h | --header-number=STYLE` 设定页眉的编号方式，默认方式`n`

* `-i | --line-increment=NUM` 设定每行行号增量值。

* `-l | --join-blank-lines=NUM` 将指定的一组空行数标记为一行。当`STYLE`为`a`时有效。

* `-n | --number-format=FORMAT` 按照FORMAT格式插入行号。

* `-p | --no-renumber` 不重置每个部分的行号。

* `-s | --number-separator=STRING` 在行号后添加STRING内容

* `-v | --starting-line-number=NUMBER` 首行起始编号。

* `-w | --number-width=NUMBER` 设定行号的左缩进宽度。

* `--help` 显示帮助信息。

* `--version` 显示版本。


__STYLE可以定义为：__
    * `a` 全部编号，包括空白行。

    * `t` 只编号非空白行。

    * `n` 完全不编号。

    * `pREGEXP` 只有匹配正则表达式的行才编号。

***

### Example

* ` nl LICENSE-2.0.txt`

    一般表示行号，不包含空行。

    ```
    root@3b4e07cd9fdd:/# nl LICENSE-2.0.txt

     1	                                 Apache License
     2	                           Version 2.0, January 2004
     3	                        http://www.apache.org/licenses/

     4	   TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION

     5	   1. Definitions.

     6	      "License" shall mean the terms and conditions for use, reproduction,
     7	      and distribution as defined by Sections 1 through 9 of this document.

     8	      "Licensor" shall mean the copyright owner or entity authorized by
     9	      the copyright owner that is granting the License.

    10	      "Legal Entity" shall mean the union of the acting entity and all
    11	      other entities that control, are controlled by, or are under common
    12	      control with that entity. For the purposes of this definition,
    13	      "control" means (i) the power, direct or indirect, to cause the
    14	      direction or management of such entity, whether by contract or
    15	      otherwise, or (ii) ownership of fifty percent (50%) or more of the
    16	      outstanding shares, or (iii) beneficial ownership of such entity.

    17	      "You" (or "Your") shall mean an individual or Legal Entity
    18	      exercising permissions granted by this License.

    19	      "Source" form shall mean the preferred form for making modifications,
    20	      including but not limited to software source code, documentation
    21	      source, and configuration files.

    22	      "Object" form shall mean any form resulting from mechanical
    23	      transformation or translation of a Source form, including but
    24	      not limited to compiled object code, generated documentation,
    25	      and conversions to other media types.

    26	      "Work" shall mean the work of authorship, whether in Source or
    27	      Object form, made available under the License, as indicated by a
    28	      copyright notice that is included in or attached to the work
    29	      (an example is provided in the Appendix below).

    30	      "Derivative Works" shall mean any work, whether in Source or Object
    31	      form, that is based on (or derived from) the Work and for which the
    32	      editorial revisions, annotations, elaborations, or other modifications
    33	      represent, as a whole, an original work of authorship. For the purposes
    34	      of this License, Derivative Works shall not include works that remain
    35	      separable from, or merely link (or bind by name) to the interfaces of,
    36	      the Work and Derivative Works thereof.

    37	      "Contribution" shall mean any work of authorship, including
    38	      the original version of the Work and any modifications or additions
    39	      to that Work or Derivative Works thereof, that is intentionally
    40	      submitted to Licensor for inclusion in the Work by the copyright owner
    41	      or by an individual or Legal Entity authorized to submit on behalf of
    42	      the copyright owner. For the purposes of this definition, "submitted"
    43	      means any form of electronic, verbal, or written communication sent
    44	      to the Licensor or its representatives, including but not limited to
    45	      communication on electronic mailing lists, source code control systems,
    46	      and issue tracking systems that are managed by, or on behalf of, the
    47	      Licensor for the purpose of discussing and improving the Work, but
    48	      excluding communication that is conspicuously marked or otherwise
    49	      designated in writing by the copyright owner as "Not a Contribution."

   ...

   140	   9. Accepting Warranty or Additional Liability. While redistributing
   141	      the Work or Derivative Works thereof, You may choose to offer,
   142	      and charge a fee for, acceptance of support, warranty, indemnity,
   143	      or other liability obligations and/or rights consistent with this
   144	      License. However, in accepting such obligations, You may act only
   145	      on Your own behalf and on Your sole responsibility, not on behalf
   146	      of any other Contributor, and only if You agree to indemnify,
   147	      defend, and hold each Contributor harmless for any liability
   148	      incurred by, or claims asserted against, such Contributor by reason
   149	      of your accepting any such warranty or additional liability.

   150	   END OF TERMS AND CONDITIONS

   151	   APPENDIX: How to apply the Apache License to your work.

   152	      To apply the Apache License to your work, attach the following
   153	      boilerplate notice, with the fields enclosed by brackets "[]"
   154	      replaced with your own identifying information. (Don't include
   155	      the brackets!)  The text should be enclosed in the appropriate
   156	      comment syntax for the file format. We also recommend that a
   157	      file or class name and description of purpose be included on the
   158	      same "printed page" as the copyright notice for easier
   159	      identification within third-party archives.

   160	   Copyright [yyyy] [name of copyright owner]

   161	   Licensed under the Apache License, Version 2.0 (the "License");
   162	   you may not use this file except in compliance with the License.
   163	   You may obtain a copy of the License at

   164	       http://www.apache.org/licenses/LICENSE-2.0

   165	   Unless required by applicable law or agreed to in writing, software
   166	   distributed under the License is distributed on an "AS IS" BASIS,
   167	   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   168	   See the License for the specific language governing permissions and
   169	   limitations under the License.
    ```

* `nl -i 2 LICENSE-2.0.txt`

    行号递增数为2.

    ```
    root@3b4e07cd9fdd:/# nl -i 2 LICENSE-2.0.txt

     1	                                 Apache License
     3	                           Version 2.0, January 2004
     5	                        http://www.apache.org/licenses/

     7	   TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION

     9	   1. Definitions.

    11	      "License" shall mean the terms and conditions for use, reproduction,
    13	      and distribution as defined by Sections 1 through 9 of this document.

    15	      "Licensor" shall mean the copyright owner or entity authorized by
    17	      the copyright owner that is granting the License.
    ...

    ```

* `nl -b a test.txt`

    标记所有行，包含空行。

    ```
    root@3b4e07cd9fdd:/# nl -b a test.txt
     1	line1
     2	line2
     3
     4
     5
     6
     7	line3
     8	line4
    ```

* `nl -b a -l 4 test.txt`

    将4行空行为一组标示为一行。

    ```
    root@3b4e07cd9fdd:/# nl -b a -l 4 test.txt
     1	line1
     2	line2



     3
     4	line3
     5	line4
    ```

* `nl -s ". " test.txt `

    在行号后面添加`. `

    ```
    root@3b4e07cd9fdd:/# nl -s ". " test.txt
     1. line1
     2. line2




     3. line3
     4. line4
    ```

* `nl -d "00" -b a test.txt`

    分割内容标记。

    ```
    root@3b4e07cd9fdd:/# nl -d "00" -b a test.txt
     1	line1
     2	line2
     3
     4


       line3
       line4
    ```

* `nl -v 3 test.txt`

    设定起始标号。

    ```
    root@3b4e07cd9fdd:/# nl -v 3 test.txt
     3	line1
     4	line2


     5	00

     6	line3
     7	line4
    ```


* `nl -w 20 test.txt`

    设定标号的宽度。

    ```
    root@3b4e07cd9fdd:/# nl -w 20 test.txt
                   1	line1
                   2	line2


                   3	00

                   4	line3
                   5	line4
    ```
