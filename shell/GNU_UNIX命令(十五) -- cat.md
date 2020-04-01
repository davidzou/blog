cat
====

### 语法

`cat [options] files`

***

### 参数

* `-b` 给非空行标行号，起始值为1.

* `-e` 显示非打印字符（请参阅`-v`选项），并在每行的末尾显示一个美元符号（`$`）。

* `-n` 标行号，起始值为1.

* `-s` 合并连续到空行为一行。见`[GNU_UNIX命令(十四) -- more](GNU_UNIX命令(十四) -- more.md)`

* `-t` 显示非打印字符（请参见`-v`选项），并将制表符显示为`^I`。

* `-u` 禁用输出缓冲。

* `-v` 显示非打印字符，使其可见。对于Control-X，控制字符打印为`^X`；删除字符（八进制0177）打印为 `^?`. 非ASCII字符打印为`M-`。

***

### Example

* `cat -b LINCESE`

    标出行号，不包含空行。

    ```
    zzw:blog zzw$ cat -b LICENSE
         1	                                 Apache License
         2	                           Version 2.0, January 2004
         3	                        http://www.apache.org/licenses/

         4	   TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION

         5	   1. Definitions.

         6	      "License" shall mean the terms and conditions for use, reproduction,
         7	      and distribution as defined by Sections 1 through 9 of this document.

         8	      "Licensor" shall mean the copyright owner or entity authorized by
         9	      the copyright owner that is granting the License.

         ...

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

* `cat -e LINCESE`

    替换非打印字符为`$`

    ```
    zzw:blog zzw$ cat -e LICENSE
                                     Apache License$
                               Version 2.0, January 2004$
                            http://www.apache.org/licenses/$
    $
       TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION$
    $
       1. Definitions.$
    $
          "License" shall mean the terms and conditions for use, reproduction,$
          and distribution as defined by Sections 1 through 9 of this document.$
    $
          "Licensor" shall mean the copyright owner or entity authorized by$
          the copyright owner that is granting the License.$
    $
          "Legal Entity" shall mean the union of the acting entity and all$
          other entities that control, are controlled by, or are under common$
          control with that entity. For the purposes of this definition,$
          "control" means (i) the power, direct or indirect, to cause the$
          direction or management of such entity, whether by contract or$
          otherwise, or (ii) ownership of fifty percent (50%) or more of the$
          outstanding shares, or (iii) beneficial ownership of such entity.$
    $
          "You" (or "Your") shall mean an individual or Legal Entity$
          exercising permissions granted by this License.$
    $
          "Source" form shall mean the preferred form for making modifications,$
          including but not limited to software source code, documentation$
          source, and configuration files.$
    $
          "Object" form shall mean any form resulting from mechanical$
          transformation or translation of a Source form, including but$
          not limited to compiled object code, generated documentation,$
          and conversions to other media types.$
    $
          "Work" shall mean the work of authorship, whether in Source or$
          Object form, made available under the License, as indicated by a$
          copyright notice that is included in or attached to the work$
          (an example is provided in the Appendix below).$
    $
          "Derivative Works" shall mean any work, whether in Source or Object$
          form, that is based on (or derived from) the Work and for which the$
          editorial revisions, annotations, elaborations, or other modifications$
          represent, as a whole, an original work of authorship. For the purposes$
          of this License, Derivative Works shall not include works that remain$
          separable from, or merely link (or bind by name) to the interfaces of,$
          the Work and Derivative Works thereof.$
    $
          "Contribution" shall mean any work of authorship, including$
          the original version of the Work and any modifications or additions$
          to that Work or Derivative Works thereof, that is intentionally$
          submitted to Licensor for inclusion in the Work by the copyright owner$
          or by an individual or Legal Entity authorized to submit on behalf of$
          the copyright owner. For the purposes of this definition, "submitted"$
          means any form of electronic, verbal, or written communication sent$
          to the Licensor or its representatives, including but not limited to$
          communication on electronic mailing lists, source code control systems,$
          and issue tracking systems that are managed by, or on behalf of, the$
          Licensor for the purpose of discussing and improving the Work, but$
          excluding communication that is conspicuously marked or otherwise$
          designated in writing by the copyright owner as "Not a Contribution."$
    $
          "Contributor" shall mean Licensor and any individual or Legal Entity$
          on behalf of whom a Contribution has been received by Licensor and$
          subsequently incorporated within the Work.$
    $
       2. Grant of Copyright License. Subject to the terms and conditions of$
          this License, each Contributor hereby grants to You a perpetual,$
          worldwide, non-exclusive, no-charge, royalty-free, irrevocable$
          copyright license to reproduce, prepare Derivative Works of,$
          publicly display, publicly perform, sublicense, and distribute the$
          Work and such Derivative Works in Source or Object form.$
    $
       3. Grant of Patent License. Subject to the terms and conditions of$
          this License, each Contributor hereby grants to You a perpetual,$
          worldwide, non-exclusive, no-charge, royalty-free, irrevocable$
          (except as stated in this section) patent license to make, have made,$
          use, offer to sell, sell, import, and otherwise transfer the Work,$
          where such license applies only to those patent claims licensable$
          by such Contributor that are necessarily infringed by their$
          Contribution(s) alone or by combination of their Contribution(s)$
          with the Work to which such Contribution(s) was submitted. If You$
          institute patent litigation against any entity (including a$
          cross-claim or counterclaim in a lawsuit) alleging that the Work$
          or a Contribution incorporated within the Work constitutes direct$
          or contributory patent infringement, then any patent licenses$
          granted to You under this License for that Work shall terminate$
          as of the date such litigation is filed.$
    $
       4. Redistribution. You may reproduce and distribute copies of the$
          Work or Derivative Works thereof in any medium, with or without$
          modifications, and in Source or Object form, provided that You$
          meet the following conditions:$
    $
          (a) You must give any other recipients of the Work or$
              Derivative Works a copy of this License; and$
    $
          (b) You must cause any modified files to carry prominent notices$
              stating that You changed the files; and$
    $
          (c) You must retain, in the Source form of any Derivative Works$
              that You distribute, all copyright, patent, trademark, and$
              attribution notices from the Source form of the Work,$
              excluding those notices that do not pertain to any part of$
              the Derivative Works; and$
    $
          (d) If the Work includes a "NOTICE" text file as part of its$
              distribution, then any Derivative Works that You distribute must$
              include a readable copy of the attribution notices contained$
              within such NOTICE file, excluding those notices that do not$
              pertain to any part of the Derivative Works, in at least one$
              of the following places: within a NOTICE text file distributed$
              as part of the Derivative Works; within the Source form or$
              documentation, if provided along with the Derivative Works; or,$
              within a display generated by the Derivative Works, if and$
              wherever such third-party notices normally appear. The contents$
              of the NOTICE file are for informational purposes only and$
              do not modify the License. You may add Your own attribution$
              notices within Derivative Works that You distribute, alongside$
              or as an addendum to the NOTICE text from the Work, provided$
              that such additional attribution notices cannot be construed$
              as modifying the License.$
    $
          You may add Your own copyright statement to Your modifications and$
          may provide additional or different license terms and conditions$
          for use, reproduction, or distribution of Your modifications, or$
          for any such Derivative Works as a whole, provided Your use,$
          reproduction, and distribution of the Work otherwise complies with$
          the conditions stated in this License.$
    $
       5. Submission of Contributions. Unless You explicitly state otherwise,$
          any Contribution intentionally submitted for inclusion in the Work$
          by You to the Licensor shall be under the terms and conditions of$
          this License, without any additional terms or conditions.$
          Notwithstanding the above, nothing herein shall supersede or modify$
          the terms of any separate license agreement you may have executed$
          with Licensor regarding such Contributions.$
    $
       6. Trademarks. This License does not grant permission to use the trade$
          names, trademarks, service marks, or product names of the Licensor,$
          except as required for reasonable and customary use in describing the$
          origin of the Work and reproducing the content of the NOTICE file.$
    $
       7. Disclaimer of Warranty. Unless required by applicable law or$
          agreed to in writing, Licensor provides the Work (and each$
          Contributor provides its Contributions) on an "AS IS" BASIS,$
          WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or$
          implied, including, without limitation, any warranties or conditions$
          of TITLE, NON-INFRINGEMENT, MERCHANTABILITY, or FITNESS FOR A$
          PARTICULAR PURPOSE. You are solely responsible for determining the$
          appropriateness of using or redistributing the Work and assume any$
          risks associated with Your exercise of permissions under this License.$
    $
       8. Limitation of Liability. In no event and under no legal theory,$
          whether in tort (including negligence), contract, or otherwise,$
          unless required by applicable law (such as deliberate and grossly$
          negligent acts) or agreed to in writing, shall any Contributor be$
          liable to You for damages, including any direct, indirect, special,$
          incidental, or consequential damages of any character arising as a$
          result of this License or out of the use or inability to use the$
          Work (including but not limited to damages for loss of goodwill,$
          work stoppage, computer failure or malfunction, or any and all$
          other commercial damages or losses), even if such Contributor$
          has been advised of the possibility of such damages.$
    $
       9. Accepting Warranty or Additional Liability. While redistributing$
          the Work or Derivative Works thereof, You may choose to offer,$
          and charge a fee for, acceptance of support, warranty, indemnity,$
          or other liability obligations and/or rights consistent with this$
          License. However, in accepting such obligations, You may act only$
          on Your own behalf and on Your sole responsibility, not on behalf$
          of any other Contributor, and only if You agree to indemnify,$
          defend, and hold each Contributor harmless for any liability$
          incurred by, or claims asserted against, such Contributor by reason$
          of your accepting any such warranty or additional liability.$
    $
       END OF TERMS AND CONDITIONS$
    $
       APPENDIX: How to apply the Apache License to your work.$
    $
          To apply the Apache License to your work, attach the following$
          boilerplate notice, with the fields enclosed by brackets "[]"$
          replaced with your own identifying information. (Don't include$
          the brackets!)  The text should be enclosed in the appropriate$
          comment syntax for the file format. We also recommend that a$
          file or class name and description of purpose be included on the$
          same "printed page" as the copyright notice for easier$
          identification within third-party archives.$
    $
       Copyright [yyyy] [name of copyright owner]$
    $
       Licensed under the Apache License, Version 2.0 (the "License");$
       you may not use this file except in compliance with the License.$
       You may obtain a copy of the License at$
    $
           http://www.apache.org/licenses/LICENSE-2.0$
    $
       Unless required by applicable law or agreed to in writing, software$
       distributed under the License is distributed on an "AS IS" BASIS,$
       WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.$
       See the License for the specific language governing permissions and$
       limitations under the License.$
    ```

* `cat -n LICENSE`

    标出行号，不论是否空行。

    ```
    zzw:blog zzw$ cat -n LICENSE
         1	                                 Apache License
         2	                           Version 2.0, January 2004
         3	                        http://www.apache.org/licenses/
         4
         5	   TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION
         6
         7	   1. Definitions.
         8
         9	      "License" shall mean the terms and conditions for use, reproduction,
        10	      and distribution as defined by Sections 1 through 9 of this document.
        11
        12	      "Licensor" shall mean the copyright owner or entity authorized by
        13	      the copyright owner that is granting the License.
        14
        15	      "Legal Entity" shall mean the union of the acting entity and all
        16	      other entities that control, are controlled by, or are under common
        17	      control with that entity. For the purposes of this definition,
        18	      "control" means (i) the power, direct or indirect, to cause the
        19	      direction or management of such entity, whether by contract or
        20	      otherwise, or (ii) ownership of fifty percent (50%) or more of the

        ...

       175
       176	   END OF TERMS AND CONDITIONS
       177
       178	   APPENDIX: How to apply the Apache License to your work.
       179
       180	      To apply the Apache License to your work, attach the following
       181	      boilerplate notice, with the fields enclosed by brackets "[]"
       182	      replaced with your own identifying information. (Don't include
       183	      the brackets!)  The text should be enclosed in the appropriate
       184	      comment syntax for the file format. We also recommend that a
       185	      file or class name and description of purpose be included on the
       186	      same "printed page" as the copyright notice for easier
       187	      identification within third-party archives.
       188
       189	   Copyright [yyyy] [name of copyright owner]
       190
       191	   Licensed under the Apache License, Version 2.0 (the "License");
       192	   you may not use this file except in compliance with the License.
       193	   You may obtain a copy of the License at
       194
       195	       http://www.apache.org/licenses/LICENSE-2.0
       196
       197	   Unless required by applicable law or agreed to in writing, software
       198	   distributed under the License is distributed on an "AS IS" BASIS,
       199	   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
       200	   See the License for the specific language governing permissions and
       201	   limitations under the License.
    ```

* `cat --version`

    显示版本。

    ```
    root@3ff83ffc7119:/# cat --version
    cat (GNU coreutils) 8.28
    Copyright (C) 2017 Free Software Foundation, Inc.
    License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.

    Written by Torbjorn Granlund and Richard M. Stallman.
    ```
