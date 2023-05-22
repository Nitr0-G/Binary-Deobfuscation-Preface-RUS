  Sample Page   Sample Page    calware - Binary Deobfuscation: Preface              

*   [index](https://calwa.re/)
*   [downloads](https://calwa.re/downloads)
*   [about](https://calwa.re/about)
*   [contact](https://calwa.re/contact)

Перевел Nitro
=============

Деобфускация бинарников
=======================

Содержание

*   Краткий экскурс в глубины деобфускации
*   Рулзы
*   The Roadmap
    *   Знакомство с "врагами"
        *   Комерс протекторы
        *   Представляющие угрозу
        *   "В благих целях"
    *   Техники, подтемы и прочая ерунда
        *   Instruction Substitution
        *   Control Flow Flattening
        *   Indirect Branches
        *   Opaque Predicates
        *   Function Inlining
        *   Function Outlining
        *   Dead Code
        *   Junk Code
        *   Constant Folding
        *   Constant Unfolding
        *   Virtualization
        *   Self Modification
        *   Jitting
        *   Commercial Obfuscations
        *   Malware Obfuscations
        *   ‘Legitimate’ Obfuscations
*   Вывод на сегодняшний день
*   Дополнительное чтиво
*   Ссылки на софт, который упоминался тута

* * *

Краткий экскурс в глубины деобфускации
--------------------------------------

Всем привет

Меня зовут Cal, и как вы прочитали вначале, это место, где я записываю свои опусы.

Я давно собирался собрать информацию об обфускации. Эта статья будет предисловием к моему путешествию в исследовании различных защитных механизмов и обфускации, которые они предоставляют.

Я искренне надеюсь, что к концу моего опуса и вы и я сможем уйти с чем-то замечательным: не только с инструментами для преодоления современных бинарных обфускаций, но и с более глубоким пониманием идей, которые их создают. Я надеюсь создать возможность манипулировать обфусцированной ерундой таким способом, который не будет привязан к определенному времени или имплементации.

![](https://calwa.re/assets/img-sep.png)

Рулзы
-----

Меня не интересует использование платных существующих инструментов. Я считаю, что это отталкивает как минимум часть читателей, и я против этого. Так что попрощайтесь со своей пиратской версией IDA и пр. дерьма - готовьтесь.

Кроме того, содержание этой серии в основном будет касаться образцов PE для Win64, но это не относится к инструментам. В попытке быть честным перед читателями и преодоления моего собственного знания бинарных файлов Linux я сделаю все возможное, чтобы написать несколько постов в этой области и смешать две темы, когда это будет уместно.

* * *

The Roadmap
===========

В качестве начального упражнения я хотел бы изучить [Obfuscator-LLVM (OLLVM)](https://github.com/obfuscator-llvm/obfuscator/wiki). Это позволит нам создать множество гибких примеров конкретной обфускации и понять, что входит в их создание и слом. Это также послужит основой для[LLVM’s](https://llvm.org/) intermediate representation (IR),) и то, как мы можем использовать его(IR) для лучшего понимания работы обфускации программ.

Как только у нас будет база, мы начнем анализировать более крупные и развитые коммерческие средства защиты. В нем(OLLVM) я надеюсь поместить методы, скрывающие функцию программы, в контекст вышеупомянутого[OLLVM](https://github.com/obfuscator-llvm/obfuscator/wiki) work.

Наконец, я надеюсь продемонстрировать результаты моего исследования, используя живые примеры. Это исследование ничего не значило бы для меня, если бы оно оставалось в рамках тщательно подобранных примеров; и это, конечно, ничего не значило бы для вас.

* * *

Знакомство с "врагами"
----------------------

Ниже приведен список средств защиты программного обеспечения, которые привлекли мое внимание. Как вы, возможно, заметили, существует довольно много совпадений с точки зрения средств защиты, которые они предоставляют. Это было сделано намеренно, поскольку я надеюсь подробнее остановиться на некоторых методах запутывания в контексте нескольких комерс протов, прежде чем сосредоточиться конкретно на одном.

### Комерс

*   Obfuscator-LLVM (OLLVM) (\*) - [https://github.com/obfuscator-llvm/obfuscator/wiki](https://github.com/obfuscator-llvm/obfuscator/wiki)
*   The Tigress C Diversifier/Obfuscator (\*) - [http://tigress.cs.arizona.edu/index.html](http://tigress.cs.arizona.edu/index.html)
*   ReWolf’s Virtualizer (\*) - [https://github.com/rwfpl/rewolf-x86-virtualizer](https://github.com/rwfpl/rewolf-x86-virtualizer)
*   ASProtect - [http://www.aspack.com/asprotect64.html](http://www.aspack.com/asprotect64.html)
*   Obsidium - [https://www.obsidium.de/show/details/en](https://www.obsidium.de/show/details/en)
*   PELock - [https://www.pelock.com/products/pelock](https://www.pelock.com/products/pelock)
*   Enigma Protector - [https://enigmaprotector.com/](https://enigmaprotector.com/)
*   CodeVirtualizer - [https://www.oreans.com/codevirtualizer.php](https://www.oreans.com/codevirtualizer.php)
*   Themida - [https://www.oreans.com/themida.php](https://www.oreans.com/themida.php)
*   VMProtect - [https://vmpsoft.com/](https://vmpsoft.com/)
*   GuardIT - [https://www.arxan.com/application-protection/desktop-server](https://www.arxan.com/application-protection/desktop-server)

Люди адекватные, не продают свою поебень

Итак, это список (в первую очередь) коммерческих протов. Это само собой разумеется, но важно отметить: в мире цифер есть и другие примеры обфусцирование, некоторые из которых никогда не обрабатывались коммерческими приложениями защиты. Не заблуждайтесь: эти примеры ни в коем случае не выходят за рамки нашего исследования. Ниже я перечислил некоторые из таких примеров.

### Представляющие угрозу

*   ZeusVM - [https://en.wikipedia.org/wiki/Zeus\_(malware)](https://en.wikipedia.org/wiki/Zeus_(malware))
*   FinSpy VM - [https://en.wikipedia.org/wiki/FinFisher](https://en.wikipedia.org/wiki/FinFisher)
*   Nymaim - [https://www.cyber.nj.gov/threat-profiles/trojan-variants/nymaim](https://www.cyber.nj.gov/threat-profiles/trojan-variants/nymaim)
*   Smoke Loader - [https://www.cyber.nj.gov/threat-profiles/trojan-variants/smoke-loader](https://www.cyber.nj.gov/threat-profiles/trojan-variants/smoke-loader)
*   Swizzor - [https://en.wikipedia.org/wiki/Swizzor](https://en.wikipedia.org/wiki/Swizzor)
*   APT10 ANEL - [https://en.wikipedia.org/wiki/Red\_Apollo](https://en.wikipedia.org/wiki/Red_Apollo)
*   xTunnel - [https://en.wikipedia.org/wiki/Fancy\_Bear](https://en.wikipedia.org/wiki/Fancy_Bear)
*   Uroburos/Turla - [https://en.wikipedia.org/wiki/Turla\_(malware)](https://en.wikipedia.org/wiki/Turla_(malware))

Когда люди упоминают обфускацию, очень легко прийти к выводу о вирусне. Однако у этой медали есть и другая сторона, и без нее наш список был бы неполным. Ниже приведены примеры "законного" обфусцирования, которые я собрал. Это компании, стремящиеся скрыть внутреннюю работу своего кода по причинам, которые варьируются от денежного стимулирования до защиты прав потребителей (по крайней мере, так они говорят).

### "В благих целях"

*   Skype - [https://www.skype.com/en/](https://www.skype.com/en/)
*   Spotify (See also [Widevine](https://en.wikipedia.org/wiki/Widevine)) - [https://www.spotify.com/us/](https://www.spotify.com/us/)
*   Adobe Photoshop - [https://www.adobe.com/products/photoshop.html](https://www.adobe.com/products/photoshop.html)
*   BattlEye - [https://www.battleye.com/](https://www.battleye.com/)
*   EasyAntiCheat - [https://www.easy.ac/en-us/](https://www.easy.ac/en-us/)
*   Windows PatchGuard/Kernel Patch Protection - [https://en.wikipedia.org/wiki/Kernel\_Patch\_Protection](https://en.wikipedia.org/wiki/Kernel_Patch_Protection)

Краткое примечание: некоторые из вышеперечисленных приложений были защищены с помощью записей [комерс проты](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#commercial-protectors) в нашем списке. В случае, если обфускация, реализованная этими приложениями, слишком сильно напоминает результаты работы наших коммерческих средств защиты, это будет указано и впоследствии опущено в будущих публикациях.

* * *

Техники, подтемы и прочая ерунда
--------------------------------

В этом разделе я собираюсь описать методы обфускации, используемые вышеупомянутыми обфускаторами, и составить таблицу, чтобы представить распространенность указанных методов.

#### Instruction Substitution

Также известное как instruction mutation,

> “...этот метод обфускации просто заключается в замене стандартных операторов (таких как addition, subtraction or boolean operators) функционально эквивалентными, но более сложными последовательностями инструкций”[1](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite12)

Хотя я считаю, что описание OLLVM этого метода отражает суть, я хотел бы немного обобщить его. Как бы ни был реализован этот метод, его основная цель заключается в изменении содержимого кода при сохранении исходной семантики. Этот метод не ограничивается числовыми операциями. До тех пор, пока результат преобразования функционально эквивалентен оригиналу, реализация полностью зависит от создателя.

Давайте рассмотрим несколько примеров того, как это может работать.

    mov eax, 1337h
            

Приведенный выше фрагмент кода на асме может быть преобразован во что-то вроде следующего:

    add eax, 266Ch
            shr eax, 1h
            inc eax
            

Хотя эти два фрагмента могут выглядеть по-разному, но они всё равно функционально эквивалентны. Ниже я включил различные фазы примера функции, поскольку она преобразуется в соответствии с instruction substitution pass в OLLVM.

![Example function C source code](https://calwa.re/assets/example_function_c_source.png "Example function C source code")

Original C Source Code

![Example function disassembly](https://calwa.re/assets/example_function_disasm.png "Example function disassembly")

Clean Function Disassembly

![Example function obfuscated disassembly](https://calwa.re/assets/example_function_mutated_disasm.png "Example function obfuscated disassembly")

Obfuscated Function Disassembly

![](https://calwa.re/assets/img-sep.png)

#### Control Flow Flattening

> Целью cfg является “полное сглаживание cfg программы”.[2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite0)

The [control flow graph](https://en.wikipedia.org/wiki/Control-flow_graph)здесь имеются в виду различные графы, сгенерированные инструментами анализа, которые показывают layout в виде [basic blocks](https://en.wikipedia.org/wiki/Basic_block).

В дальнейшем,

> “В cfg каждый узел(node) в графе представляет собой базовый блок, то есть прямолинейный фрагмент кода без каких-либо переходов или целей перехода; цели перехода начинают блок, а переходы заканчивают блок. Направленные edges используются для представления переходов в cf. В большинстве презентаций есть два специально выделенных блока: блок входа, через который управление поступает в граф потока, и блок выхода, через который весь cf выходит.”[3](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite1)

![Dummy test program C source code](https://calwa.re/assets/dummy_program_source.png "Dummy test program C source code")

The original source code

![Dummy test program assembly code](https://calwa.re/assets/dummy_program_asm.png "Dummy test program assembly code")

The disassembly for our test function

![Test function after OLLVM control flow flattening obfuscation](https://calwa.re/assets/ollvm_control_flow_flattening_pass.png "Test function after OLLVM control flow flattening obfuscation")

The disassembly for our test function after being ran through one of OLLVM's control flow flattening obfuscation passes

![](https://calwa.re/assets/img-sep.png)

#### Indirect Branches

An indirect branch,

> “...вместо указания адреса следующей инструкции для выполнения, как в direct branch, аргумент указывает, где находится адрес”[4](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite2)

Простой способ представить это - последовательность инструкций, подобная следующей:

    mov eax, function_address
            ...
            jmp eax
            

Вот пример indirect branch, взятый из одной из вышеперечисленных программ. Эта программа пронизана барчами, которые выглядят точно так же; и в то время IDA не могла следить за ними.

![indirect branch ida](https://calwa.re/assets/ida-indirect-jump.png)

![](https://calwa.re/assets/img-sep-2.png)

#### Opaque Predicates

> “an opaque predicate is a predicate—an expression that evaluates to either “true” or “false”—for which the outcome is known by the programmer … but which, for a variety of reasons, still needs to be evaluated at run time” [5](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite3)

Opaque predicates это особенно неприятная форма запутывания потока управления. Они бывают всех форм и размеров и определение opaque-_ness_ может оказаться довольно сложной задачей.

Чтобы расширить приведенное выше объяснение, в контексте обфусцирвоанного файла это выражение “true” или “false” вычисляется в форме conditional branch. Ниже я включил небольшой пример _opaque_ predicate.

    mov eax, 4h
            ...
            cmp eax, 4h
            je label_1
            mov eax, 224h ; * this will never execute
            jmp label_187 ; *
            label_1:
            mov eax, 22h
            ...
            

Теперь, как вы можете ожидать, это нереалистичный пример opaque predicate. Однако этот пример действительно иллюстрирует идею использования conditional branches для обфускации cf программы. И хотя этот пример явно довольно прост, большинство реализаций этого метода таковыми не являются. Например, приведенная выше форма opaque predicate иногда упоминается как _invariant_ opaque predicate. [6](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite15)Также есть_contextual_ opaque predicates, значение которого не является самодостаточным, но которое зависит от значения внешней переменной (invariant). [6](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite15)Также не стоит забывать о _dynamic_ opaque predicates, которые являются частью группировки предикатов, расположенных таким образом, что все возможные пути выполняют одну и ту же базовую функциональность. [6](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite15) [7](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite16)\- Все это лишь верхушка айсберга.

Opaque predicates также обычно возникают проблемы в аналитических платформах, когда делаются определенные предположения. Например, IDA Pro - это один из примеров инструмента, который ожидает хорошего, корректного кода. [8](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite17)Ниже я привел пример того, что может произойти, когда вы нарушаете предположения, сделанные в ходе бинарного анализа IDA.

![Opaque predicate assembly source code](https://calwa.re/assets/fasm_opaque_predicate.png "Opaque predicate assembly source code")

'Opaque' predicate assembly code

![IDA Pro disassembling opaque predicate](https://calwa.re/assets/ida_disassembles_opaque_predicate.png "IDA Pro disassembling opaque predicate")

IDA Pro continues to disassemble the code

![Creating an improved opaque predicate to confuse IDA Pro in FASM](https://calwa.re/assets/fasm_opaque_predicate_confuse_analysis_2.png)

Improved 'opaque' predicate assembly code

![IDA Pro confused analysis when finding our previously created opaque predicate](https://calwa.re/assets/ida_confused_analysis_2.png "IDA Pro confused analysis when finding our previously created opaque predicate")

Confusing IDA's analysis hides proceeding instructions

Как вы можете видеть в приведенной выше демонстрации, даже простые попытки отключить инструменты анализа могут значительно изменить выходные данные. Здесь IDA сделала предположение, что один фиктивный байт данных в нашем opaque predicate был [opcode expansion prefix](http://www.c-jump.com/CIS77/CPU/x86/lecture.html#X77_0040_opcode_sizes) вынуждающим команду требовать по крайней мере еще один байт. Этот следующий байт, конечно же, вставлялся в реальную инструкцию, которая должна была быть выполнена.

Если кто-то не обращал внимания, вы могли бы пропустить, что IDA сигнализировала об этом преобразовании в инструкции`jz short near ptr loc_40100E+1`где`+1`в конце внимательный реверс-инженер сообщит, что начальный бранч пропускает один байт в своем целевом местоположении. Ниже я показал очень простое исправление для этого преобразования.

![IDA Pro FASM opaque predicate confused analysis fix](https://calwa.re/assets/ida_confused_analysis_2_fix.png "IDA Pro FASM opaque predicate confused analysis fix")

Simple fix for the above transformation

![](https://calwa.re/assets/img-sep.png)

### Function Inlining

> “В нормальной терминалогии inline expansion, или inlining, - это оптимизация вручную или компилятором, которая заменяет function call site телом вызываемой функции”[9](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite4)

Function inlining - это хорошо известный метод оптимизации компилятора. [10](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite5)Вас не должно удивлять, что при неправильном использовании и в сочетании с другими нашими методами обфускации inline expansion может стать довольно эффективным средством обфускации. Возьмем следующий пример:

    int bar (int val)
            {
            	return val * 2;
            }
            
            int foo (int a, int b)
            {
            	int c = bar(b);
            	return a + c;
            }
            

Чтобы уменьшить накладные расходы при этом вызове, ваш компилятор может выполнить inline expansion и разрешить функцию`bar`будет inlined into `foo`, показано ниже:

    int foo (int a, int b)
            {
            	int c = b * 2;
            	return a + c;
            }
            

Что, конечно, можно было бы еще больше уменьшить, устранив зависимость от переменной `c`, но вы поняли суть

Теперь представьте, что вместо умножения числа на два функция`bar` отвечала за запись информации о времени выполнения в файл. В этой гипотетической функции логирования предположим, что мы также решили реализовать логику "с сюрпризом" относительно типа информации, которую мы регистрируем. Если функция `foo`вызывалась бы для логирования, даже всего пару раз, вы уже можете начать представлять, насколько наша функция расширилась бы на уровне source level; не говоря уже о дизассемблинге.

Иронично: наиболее эффективное средство упрощения inlining function также используется в качестве метода обфускации.

![](https://calwa.re/assets/img-sep-2.png)

### Function Outlining

> “Выделите фрагментов функции в их собственные функции”[11](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite6)

Синонимом function splitting является широкое определение (и дальнейшая реализация) выделения функций. Некоторые протекторы просто разделяют базовые блоки функции, которые затем сохраняют прямое отношение "один к одному" внутри своей parent функции. Некоторые другие средства защиты создают совершенно новые функции, используемые несколькими разными callers. Например, возьмем следующую диаграмму:

![function splitting asu tigress](https://calwa.re/assets/function-splitting-tigress-asu.png)

ASU's Tigress C Obfuscator "Function Splitting" Diagram ([source](http://tigress.cs.arizona.edu/transformPage/docs/split/index.html))

Приведенное выше изображение должно означать, что новые функции, `f1` и `f2`, могут содержать код, который не был изначально представлен в оригинальной функции`f`(например, пролог и эпилог). Это преобразование затем включило бы другие функции, которые могут полагаться на функциональность`f1` или `f2`, "опустить"(подразумевается как забыть) свой собственный код и вместо этого полагаться на outlined функцию.

Позже мы рассмотрим, как именно эта функция реализована в Tigress. Тем временем мы можем провести интересное сравнение с OLLVM. В отличие от подразумеваемой функции Tigress, описывающей реализацию, OLLVM ведет себя прямо противоположным образом. Дизассемблирование OLLVM сохранит прямую связь "one-to-one" со своим parent объектом в случае splitting pass.

![](https://calwa.re/assets/img-sep.png)

### Dead Code

> “…dead code - некая часть в asm коде программы, который выполняется, но результат которого никогда не используется ни в каких других вычислениях“ [12](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite13)

Кроме того, может быть важно отметить: регистры, которые мертвый код использует на протяжении всего своего жизненного цикла, иногда называются _dead registers_.!

Здесь я включил небольшой фрагмент асм кода, который включает в себя только выполняющийся код(тот код, который в случае невыполнения влияет на результаты).

    mov eax, 2h
            add eax, 2h
            cmp eax, 4h
            ...
            

Ниже я переработал наш приведенный выше пример с помощью нескольких инструкций, которые соответствуют приведенному выше определению мертвого кода. Эти инструкции отмечены звездочками.

    mov ebx, 24h ; *
            mov eax, 2h
            add ebx, 12h ; *
            add eax, 2h
            mov ecx, ebx ; *
            inc ecx ; *
            cmp eax, 4h
            ...
            

Поскольку приведенный выше мертвый код занимает регистры `ebx` и `ecx`, эти регистры теперь можно рассматривать _мертвыми_. Однако это не означает, что эти регистры всегда будут оставаться мертвыми. Занятие не-мертвыми,_living_ code, восстановит статус живых =) `ebx` и `ecx`.

![](https://calwa.re/assets/img-sep-2.png)

### Junk Code

Также известный как unreachable code,

> “… То, что никогда не выполнится, так как CF никогда этого не достигнет” [13](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite14)

Одна из вещей, которую приведенный выше фрагмент не распознает, заключается в том, что в случае opaque predicate, вторичное условие которого никогда не выполняется, будет существовать CF к его местоположению. Я создал пример этого ниже:

    mov eax, 4h
            ...
            cmp eax, 4h
            je label_1 		; (A)
            mov ecx, 1h 		; * this will never execute
            sub ebx, ecx 		; *
            push ebx 		; *
            call function_224	; *
            label_1: 		; (B)
            ...
            

Инструкции, вложенные между jcc (A) и нашей первой меткой (B), являются junk code; они никогда не будут выполнены. Однако это не останавливает существование CF к их местоположению.

![](https://calwa.re/assets/img-sep.png)

### Constant Folding

Чтобы понять идею constant unfolding (см. ниже), ты должен сначала понять идею constant folding.

> “Constant folding - это процесс распознавания и оценки постоянных выражений во время компиляции, а не вычисления их во время выполнения.” [14](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite7)

Предположим, мы начали с фрагмента кода, подобного следующему:

    int x = 220;
            int y = (x / 2) + 1;
            

Constant folding, в данном случае это просто сокращение выражения, на которое указывает наша переменная `y`во время выполнения:

    int x = 220;
            int y = 111;
            

Как указывалось выше, constant folding - это форма оптимизации, выполняемая компиляторами. Непосредственно, это не тот метод, который обычно соответствует обфускации кода!; но, учитывая контекст, constant folding вполне может быть использован как средство для скрытия функции кода; как это происходит со многими оптимизациями компилятора.

![](https://calwa.re/assets/img-sep-2.png)

### Constant Unfolding

В то время как в качестве метода обфускации можно использовать как constant folding, так и constant unfolding, чаще наблюдается constant unfolding.!

Учитывая приведенный выше пример constant folding, я сконструировал более сложный пример constant unfolding ниже. Кое-что, о чем следует помнить: хотя constant folding и unfolding кажется легким делом, не позволяйте этому ввести вас в заблуждение. Помните, что сила обычно заключается в реализации.

    int x = 220;
            int y = (((x - 0xAA) | 0x58) ^ (((((x / 4) * 0x138759) >> 0x10) & 0x11) + 0x6)) + 0x3;
            

![constant unfolding python simplification](https://calwa.re/assets/constant-unfolding-python-simplification.png)

Итак, на минутку представьте, что приведенная выше арифметика выполняется в асме, среди других операций, через flattened функцию, и все это выполняется через VM.

![](https://calwa.re/assets/img-sep.png)

### Virtualization

> “Vcpu” [15](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite8)

Это оно.

Виртуальные машины, используемые при обфускации кода, не похожи на [VMware](https://www.vmware.com/solutions/virtualization.html) или [VirtualBox](https://www.virtualbox.org/wiki/VirtualBox). У них нет подобных технологий [AMD-V](https://en.wikipedia.org/wiki/AMD-V) или [Intel VT-x](https://en.wikipedia.org/wiki/Intel_VT-x)(к счастью). Вместо этого VM, найденные в обфусцированных двоичных файлах, являются просто интерпретаторами.!

Исходный, невиртуализированный код базового файла был преобразован в пользовательский байт-код(PCODE). Этот PCODE передается интерпретатору, задачей которого является имитация действий указанного базового двоичного файла.

![CodeVirtualizer VM obfuscation transformation comparison assembly source diagram](https://calwa.re/assets/codevirtualizer_vm_transformation.jpg "CodeVirtualizer VM obfuscation transformation comparison assembly source diagram")

Orean's CodeVirtualizer vm-based protection diagram ([source](https://www.oreans.com/CodeVirtualizer.php))

Выше я прикрепил скрин работы комерс вирты[CodeVirtualizer](https://www.oreans.com/CodeVirtualizer.php). Как вы можете видеть, упрощенный код слева преобразуется в совершенно другой набор команд справа.

Я понимаю, что приведенного выше объяснения не хватает. Проблема с подробным описанием этого метода обфускации сейчас заключается в потенциальной вариативности его реализации. Итак, давайте поставим точку в этом вопросе. Я значительно подробнее остановлюсь на этой технике в следующих постах.

![](https://calwa.re/assets/img-sep.png)

### Self Modification

Это код, который изменяет сам себя, сейчас бы не хотелось говорить об этом, так как он тоже зависит от реализации

Однако мне не терпится изучить эту технику, поскольку она потребует от нас создания нескольких примеров для отработки.

![](https://calwa.re/assets/img-sep-2.png)

### Jitting

Наконец, я хочу кратко обсудить концепцию _jitting_.

> “В нормальной терминологии JIT-компиляция (также динамическая трансляция или компиляции во время выполнения) - это способ выполнения кода, который включает компиляцию во время выполнения программы - во время выполнения – а не перед выполнением”.[17](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite10)

Можно многое сказать о потенциале этой техники. Это тот, который, как я полагаю, используется только в [Tigress C Diversifier](http://tigress.cs.arizona.edu/index.html). Это также техника, которую мы будем развивать в примерах, которые мы создаем.

Что интересно так это то, что[Tigress](http://tigress.cs.arizona.edu/index.html) также включает в себя возможность реализовать преобразование, называемое _JitDynamic_, который “... похож на Jit-преобразование, за исключением того, что jit-код постоянно модифицируется и обновляется во время выполнения”[18](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite11). Тогда этот метод было бы лучше классифицировать как самоизменяющийся код.

Приведенная ниже таблица в значительной степени основана на информации, которую я прочитал у других; и процитирована соответствующим образом. Данные, в которых отсутствуют ссылки, в значительной степени спекулятивны или подкреплены проведенными мной исследованиями, которые не будут обнародованы. Приложения, методы и порядок их использования - все это может быть изменено. Со временем этот фрагмент будет заменен, и вместо существующих ниже будут использованы цитаты из моего собственного исследования. А пока отнеситесь ко всему, что приведено в таблице ниже, с недоверием и ознакомьтесь с этими цитатами (если таковые имеются).

* * *

### Commercial Obfuscations

Application

Instruction Substitution

Control Flow Modification

Indirect Branches

Opaque Predicates

Function Inlining

Function Outlining

Dead Code

Junk Code

Constant Unfolding

Virtualization

Self Modification

Jitting

Tigress

✅[19](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite18)

✅[19](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite18)

✅[19](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite18)

✅[19](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite18)

✅[19](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite18)

✅[19](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite18)

✅[19](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite18)

✅[19](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite18)

✅[19](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite18)

✅[19](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite18)

✅[19](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite18)

✅[19](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite18)

Themida

✅[20](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite19)

✅!

✅!

✅!

❌!

✅!

✅!

✅!

✅!

✅[20](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite19)

❌!

❌!

VMProtect

✅[21](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite20)

✅!

✅!

✅[21](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite20)

❔

✅[21](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite20)

✅[21](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite20)

✅[21](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite20)

✅!

✅[21](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite20)

❌!

❌!

GuardIT

❔

❔

❔

❔

❔

❔

❔

❔

❔

❔

❔

❔

PELock

✅[22](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite21)

✅[22](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite21)

✅[22](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite21)

✅[22](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite21)

❌!

✅[22](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite21)

✅[22](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite21)

✅[22](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite21)

❌!

❌!

❌!

❌!

OLLVM

✅[23](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite22)

✅[24](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite23)

❌!

✅[25](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite24)

❌!

✅[24](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite23)

❌!

✅[25](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite24)

✅[23](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite22)

❌!

❌!

❌!

CodeVirtualizer

✅[26](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite25)

❌!

❌!

❌!

❌!

❌!

❌!

❌!

❌!

✅[26](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite25)

❌!

❌!

ReWolf’s Virtualizer

❌!

❌!

❌!

❌!

❌!

❌!

❌!

❌!

❌!

✅[27](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite26)

❌!

❌!

Enigma Protector

✅[28](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite27)

❔

❔

❔

❌!

❔

❔

❔

❔

✅[28](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite27)

❌!

❌!

Obsidium

❌!

❔

❔

❔

❌!

❔

❔

❔

❔

✅[29](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite28)

❌!

❌!

ASProtect

❌!

❔

❔

✅[30](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite29)

❌!

❔

❌!

✅[30](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite29)

❔

❌!

❌!

❌!

### Malware Obfuscations

Application

Instruction Substitution

Control Flow Modification

Indirect Branches

Opaque Predicates

Function Inlining

Function Outlining

Dead Code

Junk Code

Constant Unfolding

Virtualization

Self Modification

Jitting

ANEL

❌!

✅[31](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite30)

✅[31](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite30)

✅[31](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite30)

❌!

❌!

✅[31](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite30)

✅[31](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite30)

✅[31](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite30)

❌!

❌!

❌!

Smoke Loader

❌!

✅[32](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite31)

❔

✅[32](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite31)

❌!

✅[32](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite31)

❌!

❔

❔

❌!

✅[32](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite31)

❌!

Nymaim

❌!

✅[33](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite32)

✅[33](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite32)

✅[34](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite33)

❌!

❌!

❔

❔

✅[34](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite33)

❌!

❌!

❌!

xTunnel

❌!

❔

❔

✅[35](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite34) [36](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite35)

❌!

❌!

✅[37](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite36)

✅[35](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite34)

❔

❌!

❌!

❌!

Uroburos/Turla

❌!

❔

❔

✅[38](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite37)

❌!

❌!

❔

✅[38](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite37)

❔

❌!

❌!

❌!

FinSpy VM

❌!

❔

❌!

✅[39](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite39)

❌!

❌!

✅[39](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite39)

✅!

✅[39](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite39)

✅[40](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite38) [39](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite39)

❌!

❌!

ZeusVM

❌!

❔

❔

❔

❌!

❌!

❔

❔

❔

✅[41](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite40)

❌!

❌!

Swizzor

❌!

❔

❔

❔

❌!

❌!

✅[42](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite41)

❔

❔

❌!

❌!

❌!

### ‘Legitimate’ Obfuscations

Application

Instruction Substitution

Control Flow Modification

Indirect Branches

Opaque Predicates

Function Inlining

Function Outlining

Dead Code

Junk Code

Constant Unfolding

Virtualization

Self Modification

Jitting

BattlEye

✅!

✅!

✅!

✅!

❌!

✅!

✅!

✅!

✅!

✅!

❌!

❌!

EasyAntiCheat

❔

❔

❔

❔

❔

❔

❔

❔

❔

❔

❔

❔

Photoshop

❔

❔

❔

❔

❔

❔

❔

❔

❔

❔

❔

❔

Spotify

❔

❔

❔

❔

❔

❔

❔

❔

❔

❔

❔

❔

PatchGuard

❌!

✅[43](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite42) [44](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite45)

✅[45](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite43)

❔

❌!

❌!

❔

❔

❔

❌!

✅[46](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite44)

❌!

Skype

❌!

✅[47](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite46)

✅[47](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite46)

✅[47](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fn:cite46)

❌!

❌!

❌!

❔

❔

❌!

❌!

❌!

Примечание: Во многих вышеуказанных приложениях используются различные формы модификации CF, которые не являются CF flattening. Таким образом, я обобщил эту категорию в таблице выше.

* * *

Вывод
=====

В этом посте я сделал несколько смелых и амбициозных заявлений. Системы, подобные [PatchGuard](https://en.wikipedia.org/wiki/Kernel_Patch_Protection) они мне совершенно неизвестны. Вредоносное ПО, подобное [Turla](https://en.wikipedia.org/wiki/Turla_(malware)) и [Sednit](https://en.wikipedia.org/wiki/Fancy_Bear)это спонсируемые государством угрозы с высокоприоритетными целями. Не говоря уже о том, что обфускация, используемое всеми вышеперечисленными защитами и т.п. штукой, считается сложным - очень трудным для работы материалом. Существует нехватка простых, прямолинейных решений; и исследования по разработке такого инструментария, как правило, проводятся в академических кругах.

Нет никаких сомнений в том, что предстоящий путь будет долгим и сложным. Это потребует от меня многого узнать о том, как реализуются эти методы и как мы можем лучше их обойти.

Как бы то ни было, сейчас я даю вам обещание: мое исследование **будет** применимо к современной обфускации. Я не буду отклоняться от [цели, которые я сформулировал изначально](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#goals).

![](https://calwa.re/assets/img-sep.png)

Чтиво
=====

Выше я упоминал, что наиболее заметные исследования в области деобфускации в основном проводятся в академических кругах. Обычно эти ресурсы сухие, и их трудно прочесть. Итак, чтобы не утомлять вас скучными и умозрительными исследованиями, ниже я сделал все возможное, чтобы перечислить ресурсы, которые, по моему мнению, были практичными и привлекательными.

*   _Diablo - Deobfuscation: By Hand_ ([https://diablo.elis.ugent.be/node/54](https://diablo.elis.ugent.be/node/54))
*   _The Tigress C Diversifier/Obfuscator: Transformations_ ([http://tigress.cs.arizona.edu/transformPage/index.html](http://tigress.cs.arizona.edu/transformPage/index.html))
*   _Intermediate Representation - Wikipedia_ ([https://en.wikipedia.org/wiki/Intermediate\_representation](https://en.wikipedia.org/wiki/Intermediate_representation))
*   _Usenix - Disassembling Obfuscated Binaries_ ([https://www.usenix.org/legacy/publications/library/proceedings/sec04/tech/full\_papers/kruegel/kruegel\_html/node3.html](https://www.usenix.org/legacy/publications/library/proceedings/sec04/tech/full_papers/kruegel/kruegel_html/node3.html))
*   _Optimizing Compiler (Specific Techniques) - Wikipedia_ ([https://en.wikipedia.org/wiki/Optimizing\_compiler#Specific\_techniques](https://en.wikipedia.org/wiki/Optimizing_compiler#Specific_techniques))
*   _Compiler Optimizations for Reverse Engineers - Rolf Rolles, Mobius Strip Reverse Engineering_ ([https://www.msreverseengineering.com/blog/2014/6/23/compiler-optimizations-for-reverse-engineers](https://www.msreverseengineering.com/blog/2014/6/23/compiler-optimizations-for-reverse-engineers))
*   _Udupa, Sharath K., Saumya K. Debray, and Matias Madou. “Deobfuscation: Reverse engineering obfuscated code.” 12th Working Conference on Reverse Engineering (WCRE’05). IEEE, 2005._ ([https://ieeexplore.ieee.org/abstract/document/1566145](https://ieeexplore.ieee.org/abstract/document/1566145))
*   _Deobfuscation: recovering an OLLVM-protected program_ ([https://blog.quarkslab.com/deobfuscation-recovering-an-ollvm-protected-program.html](https://blog.quarkslab.com/deobfuscation-recovering-an-ollvm-protected-program.html))
*   _Aspire - Publications_ ([https://aspire-fp7.eu/papers](https://aspire-fp7.eu/papers))
*   _Creating Code Obfuscation Virtual Machines - RECon 2008_ ([https://www.youtube.com/watch?v=d\_OFrP-m2xU](https://www.youtube.com/watch?v=d_OFrP-m2xU)[](http://savefrom.net/?url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3Dd_OFrP-m2xU&utm_source=opera-chromium&utm_medium=extensions&utm_campaign=link_modifier "Получи прямую ссылку"))

* * *

References
==========

1.  _Obfuscator-LLVM: Instruction Substitution_ - [https://github.com/obfuscator-llvm/obfuscator/wiki/Instructions-Substitution](https://github.com/obfuscator-llvm/obfuscator/wiki/Instructions-Substitution) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite12)
    
2.  _Control Flow Flattening (Wikipedia)_ - [https://github.com/obfuscator-llvm/obfuscator/wiki/Control-Flow-Flattening](https://github.com/obfuscator-llvm/obfuscator/wiki/Control-Flow-Flattening) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite0)
    
3.  _\[PDF\] Masking wrong-successor Control Flow Errors employing data redundancy_ - [https://ieeexplore.ieee.org/abstract/document/7365827](https://ieeexplore.ieee.org/abstract/document/7365827) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite1)
    
4.  _Indirect Branch (Wikipedia)_ - [https://en.wikipedia.org/wiki/Indirect\_branch](https://en.wikipedia.org/wiki/Indirect_branch) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite2)
    
5.  _Opaque Predicate (Wikipedia)_ - [https://en.wikipedia.org/wiki/Opaque\_predicate](https://en.wikipedia.org/wiki/Opaque_predicate) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite3)
    
6.  _Thomas Ridma. “Seeing through obfuscation: interactive detection and removal of opaque predicates.” In the Digital Security Group, Institute for Computing and Information Sciences. 2017._ - [https://th0mas.nl/downloads/thesis/thesis.pdf](https://th0mas.nl/downloads/thesis/thesis.pdf) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite15) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite15:1) [↩3](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite15:2)
    
7.  _Palsberg, Jens, et al. “Experience with software watermarking.” Proceedings 16th Annual Computer Security Applications Conference (ACSAC’00). IEEE, 2000._ - [https://ieeexplore.ieee.org/document/898885](https://ieeexplore.ieee.org/document/898885) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite16)
    
8.  _Hex-Rays: IDA and obfuscated code_ - [https://www.hex-rays.com/products/ida/support/ppt/caro\_obfuscation.ppt](https://www.hex-rays.com/products/ida/support/ppt/caro_obfuscation.ppt) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite17)
    
9.  _Inline Expansion (Wikipedia)_ - [https://en.wikipedia.org/wiki/Inline\_expansion](https://en.wikipedia.org/wiki/Inline_expansion) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite4)
    
10.  _Eilam, Eldad. \*Reversing: Secrets of Reverse Engineering\*. Wiley, 2005. Print._ - [https://www.wiley.com/en-us/Reversing%3A+Secrets+of+Reverse+Engineering+-p-9780764574818](https://www.wiley.com/en-us/Reversing%3A+Secrets+of+Reverse+Engineering+-p-9780764574818) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite5)
    
11.  _The Tigress C Diversifier/Obfuscator: Function Splitting_ - [http://tigress.cs.arizona.edu/transformPage/docs/split/index.html](http://tigress.cs.arizona.edu/transformPage/docs/split/index.html) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite6)
    
12.  _Dead Code (Wikipedia)_ - [https://en.wikipedia.org/wiki/Dead\_code](https://en.wikipedia.org/wiki/Dead_code) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite13)
    
13.  _Unreachable Code (Wikipedia)_ - [https://en.wikipedia.org/wiki/Unreachable\_code](https://en.wikipedia.org/wiki/Unreachable_code) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite14)
    
14.  _Constant Folding (Wikipedia)_ - [https://en.wikipedia.org/wiki/Constant\_folding](https://en.wikipedia.org/wiki/Constant_folding) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite7)
    
15.  _Virtual Machine (Wikipedia)_ - [https://en.wikipedia.org/wiki/Virtual\_machine](https://en.wikipedia.org/wiki/Virtual_machine) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite8)
    
16.  _Self-Modifying Code (Wikipedia)_ - [https://en.wikipedia.org/wiki/Self-modifying\_code](https://en.wikipedia.org/wiki/Self-modifying_code) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite9)
    
17.  _Just-in-time Compilation (Wikipedia)_ - [https://en.wikipedia.org/wiki/Just-in-time\_compilation](https://en.wikipedia.org/wiki/Just-in-time_compilation) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite10)
    
18.  _The Tigress C Diversifier/Obfuscator: Dynamic Obfuscation_ - [http://tigress.cs.arizona.edu/transformPage/docs/jitDynamic/index.html](http://tigress.cs.arizona.edu/transformPage/docs/jitDynamic/index.html) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite11)
    
19.  _The Tigress C Diversifier/Obfuscator: Transformations_ - [http://tigress.cs.arizona.edu/transformPage/index.html](http://tigress.cs.arizona.edu/transformPage/index.html) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite18) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite18:1) [↩3](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite18:2) [↩4](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite18:3) [↩5](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite18:4) [↩6](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite18:5) [↩7](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite18:6) [↩8](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite18:7) [↩9](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite18:8) [↩10](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite18:9) [↩11](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite18:10) [↩12](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite18:11)
    
20.  _Oreans: Themida Overview_ - [https://www.oreans.com/Themida.php](https://www.oreans.com/Themida.php) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite19) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite19:1)
    
21.  _VMProtect Software Protection: What is VMProtect?_ - [http://vmpsoft.com/support/user-manual/introduction/what-is-vmprotect/](http://vmpsoft.com/support/user-manual/introduction/what-is-vmprotect/) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite20) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite20:1) [↩3](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite20:2) [↩4](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite20:3) [↩5](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite20:4) [↩6](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite20:5)
    
22.  _PELock: Software protection system_ - [https://www.pelock.com/products/pelock](https://www.pelock.com/products/pelock) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite21) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite21:1) [↩3](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite21:2) [↩4](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite21:3) [↩5](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite21:4) [↩6](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite21:5) [↩7](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite21:6)
    
23.  _Obfuscator-LLVM: Instruction Substitution_ - [https://github.com/obfuscator-llvm/obfuscator/wiki/Instructions-Substitution](https://github.com/obfuscator-llvm/obfuscator/wiki/Instructions-Substitution) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite22) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite22:1)
    
24.  _Obfuscator-LLVM: Control Flow Flattening_ - [https://github.com/obfuscator-llvm/obfuscator/wiki/Control-Flow-Flattening](https://github.com/obfuscator-llvm/obfuscator/wiki/Control-Flow-Flattening) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite23) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite23:1)
    
25.  _Obfuscator-LLVM: Bogus Control Flow_ - [https://github.com/obfuscator-llvm/obfuscator/wiki/Bogus-Control-Flow](https://github.com/obfuscator-llvm/obfuscator/wiki/Bogus-Control-Flow) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite24) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite24:1)
    
26.  _Oreans: CodeVirtualizer Overview_ - [https://oreans.com/CodeVirtualizer.php](https://oreans.com/CodeVirtualizer.php) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite25) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite25:1)
    
27.  _\[PDF\] ReWolf’s x86 Virtualizer: Documentation_ - [http://rewolf.pl/stuff/x86.virt.pdf](http://rewolf.pl/stuff/x86.virt.pdf) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite26)
    
28.  _The Enigma Protector: About_ - [https://enigmaprotector.com/en/about.html](https://enigmaprotector.com/en/about.html) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite27) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite27:1)
    
29.  _Obsidium: Product Information_ - [https://www.obsidium.de/show/details/en](https://www.obsidium.de/show/details/en) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite28)
    
30.  _David, Robin, Sébastien Bardin, and Jean-Yves Marion. “Targeting Infeasibility Questions on Obfuscated Codes.” arXiv preprint arXiv:1612.05675 (2016)._ - [https://arxiv.org/pdf/1612.05675](https://arxiv.org/pdf/1612.05675) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite29) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite29:1)
    
31.  _Carbon Black: Defeating Compiler-Level Obfuscations Used in APT10 Malware_ - [https://www.carbonblack.com/2019/02/25/defeating-compiler-level-obfuscations-used-in-apt10-malware/](https://www.carbonblack.com/2019/02/25/defeating-compiler-level-obfuscations-used-in-apt10-malware/) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite30) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite30:1) [↩3](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite30:2) [↩4](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite30:3) [↩5](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite30:4) [↩6](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite30:5)
    
32.  _CERT.PL: Dissecting Smoke Loader_ - [https://www.cert.pl/en/news/single/dissecting-smoke-loader/](https://www.cert.pl/en/news/single/dissecting-smoke-loader/) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite31) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite31:1) [↩3](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite31:2) [↩4](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite31:3)
    
33.  _ESET: Dymaim Obfuscation Chronicles_ - [https://www.welivesecurity.com/2013/08/26/nymaim-obfuscation-chronicles/](https://www.welivesecurity.com/2013/08/26/nymaim-obfuscation-chronicles/) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite32) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite32:1)
    
34.  _CERT.PL: Nymaim Revisited_ - [https://www.cert.pl/en/news/single/nymaim-revisited/](https://www.cert.pl/en/news/single/nymaim-revisited/) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite33) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite33:1)
    
35.  _\[PDF\] ESET: En Route with Sednit (Part 2)_ - [https://www.welivesecurity.com/wp-content/uploads/2016/10/eset-sednit-part-2.pdf](https://www.welivesecurity.com/wp-content/uploads/2016/10/eset-sednit-part-2.pdf) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite34) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite34:1)
    
36.  _\[PDF\] Sebastien Bardin, Robin David, Jean-Yves Marion. “Deobfuscation: Semantic Analysis to the Rescue”. Presentation at Virus Bulletin (2017)._ - [https://www.virusbulletin.com/uploads/pdf/conference\_slides/2017/Bardin-VB2017-Deobfuscation.pdf](https://www.virusbulletin.com/uploads/pdf/conference_slides/2017/Bardin-VB2017-Deobfuscation.pdf) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite35)
    
37.  _\[PDF\] Robin David, Sebastien Bardin. “Code Deobfuscation: Intertwining Dynamic, Static and Symbolic Approaches”. Presentation at BlackHat Europe (2016)._ - [https://www.robindavid.fr/publications/BHEU16\_Robin\_David.pdf](https://www.robindavid.fr/publications/BHEU16_Robin_David.pdf) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite36)
    
38.  _\[PDF\] ESET: Diplomats in Easter Europe Bitten by a Turla Mosquito_ - [https://www.eset.com/me/whitepapers/eset-turla-mosquito/](https://www.eset.com/me/whitepapers/eset-turla-mosquito/) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite37) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite37:1)
    
39.  _Mobius Strip Reverse Engineering: A Walk-Through Tutorial, with Code, on Statically Unpacking the FinSpy VM (Part One, x86 Deobfuscation)_ - [https://www.msreverseengineering.com/blog/2018/1/23/a-walk-through-tutorial-with-code-on-statically-unpacking-the-finspy-vm-part-one-x86-deobfuscation](https://www.msreverseengineering.com/blog/2018/1/23/a-walk-through-tutorial-with-code-on-statically-unpacking-the-finspy-vm-part-one-x86-deobfuscation) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite39) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite39:1) [↩3](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite39:2) [↩4](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite39:3)
    
40.  _Github: Rofl Rolles’ Static Unpacker for FinSpy VM_ - [https://github.com/RolfRolles/FinSpyVM](https://github.com/RolfRolles/FinSpyVM) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite38)
    
41.  _Miasm’s Blog: ZeusVM Analysis_ - [https://miasm.re/blog/2016/09/03/zeusvm\_analysis.html](https://miasm.re/blog/2016/09/03/zeusvm_analysis.html) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite40)
    
42.  _Pierre-Marc Bureau, Joan Calvet. “Understanding Swizzor’s Obfuscation Scheme”. Presentation at RECon (2010)._ - [https://archive.org/details/UnderstandingSwizzorsObfuscationScheme-Pierre-marcBureauAndJoanCalvet](https://archive.org/details/UnderstandingSwizzorsObfuscationScheme-Pierre-marcBureauAndJoanCalvet) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite41)
    
43.  _CSDN iiprogram’s Blog: Pathguard Reloaded, A Brief Analysis of PatchGuard Version 3_ - [https://blog.csdn.net/iiprogram/article/details/2456658](https://blog.csdn.net/iiprogram/article/details/2456658) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite42)
    
44.  _Uninformed: Subverting Patchguard Version 2; Obfuscation of System Integrity Check Calls via Structured Exception Handling_ - [http://uninformed.org/index.cgi?v=6&a=1&p=8](http://uninformed.org/index.cgi?v=6&a=1&p=8) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite45)
    
45.  _Uninformed: Subverting Patchguard Version 2; Anti-Debug Code During Initialization_ - [http://uninformed.org/index.cgi?v=6&a=1&p=5](http://uninformed.org/index.cgi?v=6&a=1&p=5) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite43)
    
46.  _Uninformed: Subverting Patchguard Version 2; Overwriting PatchGuard Initialization Code Post Boot_ - [http://uninformed.org/index.cgi?v=6&a=1&p=12](http://uninformed.org/index.cgi?v=6&a=1&p=12) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite44)
    
47.  _\[PDF\] Philippe Biondi, Fabrice Desclaux. “Silver Needle in the Skype”. Presentation at BlackHat Europe (2006)._ - [https://www.blackhat.com/presentations/bh-europe-06/bh-eu-06-biondi/bh-eu-06-biondi-up.pdf](https://www.blackhat.com/presentations/bh-europe-06/bh-eu-06-biondi/bh-eu-06-biondi-up.pdf) [↩](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite46) [↩2](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite46:1) [↩3](https://calwa.re/reversing/obfuscation/binary-deobfuscation-preface#fnref:cite46:2)
    

[Check out my Github](https://github.com/calware)
