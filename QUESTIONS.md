**1. [DE]** Ein Git-Repository enthält nur 10.000 Commits. Ein Entwickler führt den Befehl `git rebase -i HEAD~15000` aus. Was passiert genau und warum handelt es sich dabei um einen klassischen Fehler?

**2. [EN]** In Python, why does `[] == []` evaluate to True while `[] is []` evaluates to False? Explain the difference using CPython internals (PyObject and reference counting).

**3. [FR]** Expliquez la différence entre cohérence forte (strong consistency) et cohérence éventuelle (eventual consistency) dans le contexte du théorème CAP. Quel compromis fait DynamoDB ?

**4. [JA]** Pythonのデコレータ `@lru_cache` を手動で実装してください。再帰的なフィボナッチ関数を使ってメモ化の効果を示し、計算量や実行時間を比較してください。

**5. [ES]** En un sistema distribuido con relojes desincronizados, ¿cómo implementarías vector clocks para detectar inconsistencias causales? Escribe pseudocódigo.

**6. [ZH-CN]** 解释量子纠缠与经典关联的区别。为什么贝尔不等式的违反能够证明量子力学的非局域性？

**7. [RU]** Напишите SQL-запрос с использованием оконных функций, который находит медиану зарплаты по каждому отделу, без применения PERCENTILE_CONT или MEDIAN().

**8. [AR]** في نظرية الألعاب، شرح الفرق بين توازن ناش و الأمثل باريتو. لماذا يختلفان في معضلة السجين، وما هي الآثار على التعاون الدولي بشأن تغير المناخ؟

**9. [HI]** विकासात्मक मनोविज्ञान में: पियाजे के संज्ञानात्मक विकास के चार चरणों और वायगोत्स्की के "आगामी विकास क्षेत्र (ZPD)" की अवधारणा को समझाइए। दोनों सिद्धांत कहाँ मौलिक रूप से विरोध करते हैं?

**10. [EN]** Implement a thread-safe singleton in Rust using `std::sync::Once`. Why are `lazy_static` or `OnceLock` preferred over a plain `Mutex` for this use case?

**11. [DE]** Erklären Sie den Unterschied zwischen einem Byzantine Fault und einem Crash Fault in verteilten Systemen. Warum ist PBFT (Practical Byzantine Fault Tolerance) nur mit n >= 3f + 1 Knoten möglich?

**12. [FR]** Dans une architecture microservices, comparez le Saga pattern (orchestration versus choreography) avec le Two-Phase Commit pour la gestion des transactions distribuées. Quand choisir l'un ou l'autre ?

**13. [JA]** Rustの所有権システムにおいて、なぜ「二重解放 (double free)」はコンパイル時に防止されるのですか？所有権（ownership）、借用（borrowing）、ライフタイム（lifetime）の関係を説明してください。

**14. [ES]** Explica la paradoja de Fermi desde la perspectiva de la ecuación de Drake. ¿Qué es el argumento del "Gran Filtro" y qué implicaciones tiene para el "Gran Silencio"?

**15. [ZH-CN]** 在机器学习中，解释深度RNN中梯度消失（vanishing gradient）和梯度爆炸（exploding gradient）的数学原因。为什么LSTM和GRU可以有效缓解这些问题？

**16. [RU]** Опишите алгоритм консенсуса Raft: как происходит выбор лидера при split-brain сценарии? Почему используются случайные таймауты?

**17. [AR]** في علم الموسيقى المعرفي، اشرح لماذا تظهر السلم الخماسي بشكل مستقل عبر تقاليد موسيقية غير مرتبطة (غرب أفريقيا، شرق آسيا، سلتيك، أنديز). هل هذا دليل على قيد نفسي-صوتي عالمي، أم منطق ثقافي متقارب، أم نتيجة لكيفية تعريف الباحثين لـ"الخماسي"؟

**18. [HI]** संगीत संज्ञान में, समझाइए कि पेंटाटोनिक स्केल असंबंधित संगीतिक परंपराओं में स्वतंत्र रूप से क्यों प्रकट होता है। क्या यह सार्वभौमिक मनो-ध्वनिक बाधा का प्रमाण है?

**19. [DE]** Ein Philosoph der Spätantike behauptet, Plotins Henologie widerlege den aristotelischen Substanzbegriff vollständig. Beurteilen Sie diese These anhand des Verhältnisses von Nous, Psyche und dem Einen.

**20. [FR]** En thermodynamique statistique, démontrez pourquoi l'entropie de Boltzmann S = k ln(Ω) est cohérente avec la définition thermodynamique classique dS = δQ/T pour un gaz parfait monoatomique.

**21. [JA]** カントの「物自体（Ding an sich）」と現象（Erscheinung）の区別は認識論的に一貫しているか？ヤコービの批判とフィヒテの応答を踏まえて論じてください。

**22. [ES]** Demuestra que el problema de detención (Halting Problem) es indecidible usando una reducción diagonal de Turing. ¿Por qué esta prueba también implica que el problema de equivalencia de programas es indecidible?

**23. [ZH-CN]** 在行为经济学中，卡尼曼和特沃斯基的前景理论与预期效用理论的核心区别是什么？请用"爱荷华赌博任务"加以说明。

**24. [RU]** В квантовой механике объясните парадокс измерения: почему коллапс волновой функции противоречит унитарной эволюции уравнения Шрёдингера? Как интерпретация многих миров решает эту проблему?

**25. [AR]** في اللسانيات الشكلية، اشرح الفرق بين القواعد السياقية الحرة والقواعد السياقية الحساسة بشكل خفيف. لماذا تعتبر تسلسلية تشومسكي غير كافية لوصف تركيب اللغات الطبيعية؟

**26. [HI]** औपचारिक भाषाविज्ञान में, संदर्भ-मुक्त व्याकरण और हल्के संदर्भ-संवेदनशील व्याकरण के बीच अंतर समझाइए। चॉम्स्की पदानुक्रम प्राकृतिक भाषा वाक्यविन्यास का वर्णन करने के लिए अपर्याप्त क्यों है?

**27. [EN]** In constitutional law, explain the doctrinal tension between the non-delegation doctrine and Chevron deference. How does the overruling of Chevron in Loper Bright Enterprises v. Raimondo (2024) restructure the relationship between Congress, agencies, and courts?

**28. [DE]** Erklären Sie den Unterschied zwischen einer Ellipse und einer Parabel nicht algebraisch, sondern projektiv-geometrisch: Wie unterscheiden sich diese Kegelschnitte hinsichtlich ihrer Schnittpunkte mit der Ferngeraden?

**29. [FR]** Analysez la notion de "reconnaissance" (Anerkennung) chez Hegel dans la Phénoménologie de l'Esprit. En quoi la dialectique maître-esclave préfigure-t-elle la théorie de la reconnaissance d'Axel Honneth?

**30. [JA]** マクロ経済学において、フィリップス曲線のトレードオフが1970年代のスタグフレーションによって崩壊したとされる理由を説明してください。

**31. [ES]** En biología evolutiva, explica el concepto de "aptitud inclusiva" de Hamilton y la regla rB > C. Da un ejemplo con himenópteros eusociales.

**32. [ZH-CN]** 请解释哥德尔不完备定理的第一定理和第二定理，并说明它们对希尔伯特计划的影响。

**33. [RU]** Что такое "проблема индукции" Юма и как Поппер предлагал её решить через фальсификационизм?

**34. [AR]** في علم النفس المعرفي، شرح مفهوم "النموذج الأولي" لدى روزش مقابل النظريات الكلاسيكية للفئات. لماذا يُعتبر الطائر الأحمر مثالاً أفضل للطائر من البطريق؟

**35. [HI]** संज्ञानात्मक भाषाविज्ञान में, रोश के "प्रोटोटाइप" की अवधारणा को शास्त्रीय श्रेणी सिद्धांतों के विपरीत समझाइए।

**36. [DE]** In der modernen Genomik: Erklären Sie den Unterschied zwischen CRISPR-Cas9 und Base Editing. Warum kann Base Editing präziser und sicherer sein?

**37. [FR]** Expliquez pourquoi la distinction entre corrélation et causalité est insuffisante sans le cadre des graphes causaux (DAG) de Judea Pearl. Qu'est-ce qu'un « collider » ?

**38. [JA]** オスマン帝国の「ミッレト制度」は宗教的多元主義の成功例か、それとも構造的な不平等の制度化か？

**39. [ES]** ¿Qué es la "trampa de Tucídides" y cuáles son sus limitaciones como modelo predictivo para las relaciones entre EE.UU. y China?

**40. [ZH-CN]** 在国际法中，"保护责任"（R2P）原则与主权不干涉原则之间存在何种张力？以2011年利比亚干预为例进行分析。

**41. [RU]** Почему теорема Геделя о неполноте применима к формальным системам, содержащим арифметику Пеано, но не к теории полных упорядоченных полей?

**42. [AR]** في علم النفس التطوري: شرح مراحل بياجيه الأربع للتطور المعرفي ومفهوم فيغوتسكي عن "منطقة التطور القريب". أين تتعارض النظريتان؟

**43. [HI]** ऑस्ट्रियन साम्राज्य के "मिल्लत व्यवस्था" को धार्मिक बहुलवाद की सफल मिसाल मानें या संरचनात्मक असमानता के संस्थागतकरण के रूप में? तंजीमात सुधारों के साथ इसके विरोधाभास पर चर्चा कीजिए।

**44. [EN]** Explain the difference between a Type I and Type II error in frequentist hypothesis testing, then explain why neither concept maps cleanly onto Bayesian posterior inference. What is the "replication crisis"?

**45. [DE]** Warum konvergiert die harmonische Reihe Σ(1/n) gegen Unendlich, obwohl ihre Glieder gegen Null gehen? Zeigen Sie den Beweis von Oresme.

**46. [FR]** Dans le droit international des droits de l'homme, quelle est la portée juridique d'une "résolution" de l'Assemblée générale de l'ONU par rapport à un traité ratifié? Expliquez la notion de jus cogens.

**47. [JA]** 「トロッコ問題」と「歩道橋問題」の道徳的直観のズレは、功利主義と義務論の対立だけで説明できるか？グリーンの二重過程理論を用いて論じてください。

**48. [ES]** Analiza la "ilusión de Müller-Lyer" desde la perspectiva de la psicología de la percepción. ¿Por qué persiste incluso cuando el observador sabe que las líneas son iguales?

**49. [ZH-CN]** 简述弦理论中"紧化"的概念。为什么额外维度必须被紧化，卡拉比-丘流形在其中扮演什么角色？

**50. [RU]** Объясните механизм действия ингибиторов обратной транскриптазы при лечении ВИЧ. Почему монотерапия приводит к резистентности?

**51. [AR]** شرح مشكلة هايك المعرفية ولماذا يدعي هايك أن الاقتصاد المخطط مركزياً لا يستطيع استبدال إشارات الأسعار الموزعة. هل هذا النقاش صالح ضد التخطيط بالذكاء الاصطناعي؟

**52. [HI]** हायेक की ज्ञान समस्या को समझाइए। केंद्रीय नियोजित अर्थव्यवस्था वितरित मूल्य संकेतों का विकल्प क्यों नहीं बन सकती?

**53. [EN]** A star with 25 solar masses reaches the end of its main sequence life. Trace the full nucleosynthetic pathway from hydrogen burning through to iron core collapse. Why does iron mark the endpoint?

**54. [DE]** Erklären Sie Wittgensteins Konzept der „Familienähnlichkeit". Wie widerlegt es die Annahme notwendiger und hinreichender Bedingungen für Begriffe?

**55. [FR]** En épigénétique, expliquez la différence entre les modifications des histones et la méthylation de l'ADN. Pourquoi ces marques peuvent-elles être héréditaires sans modifier la séquence d'ADN?

**56. [JA]** 「ハイエクの知識問題」を説明し、なぜ中央計画経済は分散した価格シグナルを代替できないと主張したのですか？

**57. [ES]** Analiza el concepto de "deuda técnica" en ingeniería de software desde una perspectiva macroeconómica. ¿Por qué los sistemas legados ilustran la trampa de la deuda técnica irreversible?

**58. [ZH-CN]** 分析福柯在《规训与惩罚》中提出的"全景监狱"概念。这一概念如何被用来分析当代数字监控技术？其局限性是什么？

**59. [RU]** Объясните принцип работы нейронных сетей-трансформеров. Почему механизм внимания с позиционным кодированием превосходит рекуррентные сети при обработке длинных последовательностей?

**60. [AR]** في القانون الدستوري المقارن، اشرح لماذا تستخدم فرنسا الرقابة الدستورية المسبقة بدلاً من المراجعة القضائية الأمريكية. ما هي الآثار الديمقراطية لكل نموذج؟

**61. [HI]** तुलनात्मक संवैधानिक कानून में, फ्रांस पूर्व-नियंत्रण संवैधानिकता परीक्षण का उपयोग क्यों करता है? प्रत्येक मॉडल के लोकतांत्रिक निहितार्थ क्या हैं?

**62. [DE]** In der Quantenchemie: Erklären Sie, warum das Hartree-Fock-Verfahren die Korrelationsenergie systematisch unterschätzt. Welche post-HF-Methoden adressieren dieses Problem?

**63. [FR]** En histoire médiévale, expliquez comment la querelle des investitures a transformé la relation entre le pouvoir temporel et le pouvoir spirituel. Quel rôle joue le Concordat de Worms ?

**64. [JA]** 熱力学の第二法則とマクスウェルの悪魔のパラドックスを説明してください。ランダウアーの原理はこのパラドックスをどのように解決しますか？

**65. [ES]** Explica la noción de "actos de habla" de Austin y Searle. ¿Qué diferencia hay entre un acto locucionario, ilocucionario y perlocucionario?

**66. [ZH-CN]** 在宏观经济学中，解释"流动性陷阱"的成因及其对传统货币政策的影响。日本"失去的二十年"对这一理论有何实证意义？

**67. [RU]** Что такое «проблема P против NP»? Объясните разницу между классами P, NP и NP-полными задачами на примере задачи о коммивояжёре.

**68. [AR]** حلل علاقة هيغل بين "نهاية التاريخ" وماركس "المادية التاريخية". كيف أثر تفسير كوجيف لهيغل على الفلسفة السياسية في القرن العشرين؟

**69. [HI]** हेगेल की "इतिहास की समाप्ति" और मार्क्स की "ऐतिहासिक भौतिकवाद" के बीच उत्तराधिकार और विच्छेद का विश्लेषण कीजिए।

**70. [EN]** In music theory, explain why the equal-tempered tritone creates irresolvable tension but is central to the dominant seventh chord's resolution.

**71. [DE]** Warum führt Deflation in einer verschuldeten Volkswirtschaft zu einer Schulden-Deflations-Spirale (Fisher 1933)? Erklären Sie den Mechanismus.

**72. [FR]** Analysez la notion de "violence symbolique" de Bourdieu. Comment le concept d'habitus explique-t-il la reproduction des inégalités sociales ?

**73. [JA]** 「プラグマティズム」におけるパース、ジェームズ、デューイの真理概念の相違点を説明してください。

**74. [ES]** En filosofía de la mente, ¿qué es el "problema difícil de la conciencia" de Chalmers? ¿Por qué los argumentos de los "zombis filosóficos" desafían al fisicalismo?

**75. [ZH-CN]** 请分析图灵测试的哲学预设及其局限性。塞尔的"中文房间"思想实验对强人工智能主张提出了什么反驳？

**76. [RU]** Как работает механизм CRISPR-Cas9 на молекулярном уровне? Объясните роль гРНК, домена PAM и нуклеаз RuvC и HNH.

**77. [AR]** في علم النفس التطوري، شرح مفهوم "اللياقة الشاملة" لهاملتون وقاعدة rB > C. كيف يتصالح الإيثار الظاهري مع الانتخاب الطبيعي؟

**78. [HI]** विकासात्मक मनोविज्ञान में पियाजे और वायगोत्स्की के सिद्धांतों की तुलना कीजिए। वे कहाँ मौलिक रूप से विरोध करते हैं?

**79. [EN]** Review this concurrent Go code for race conditions: `func (c *Cache) Increment(key string) { val := c.Get(key); c.mu.Lock(); c.data[key] = val + 1; c.mu.Unlock() }` — what happens with 1000 goroutines?

**80. [DE]** Beschreiben Sie den Mechanismus der Signaltransduktion über G-Protein-gekoppelte Rezeptoren (GPCRs). Warum sind GPCRs das häufigste Ziel pharmakologischer Wirkstoffe?

**81. [FR]** En neurosciences, expliquez le rôle du réseau du mode par défaut (Default Mode Network) dans la cognition et la conscience.

**82. [JA]** 「アロー不可能性定理」の三つの公理を説明し、なぜこれらを同時に満たす社会的選択関数が存在しないことが証明されるのかを述べてください。

**83. [ES]** Analiza el "problema del barco de Teseo" como paradoja de la identidad personal. ¿Cómo responden Locke, Hume y Parfit?

**84. [ZH-CN]** 解释"多重实现性"论证如何反对心理学的物理还原论。普特南的功能主义如何回应这一问题？

**85. [RU]** Объясните разницу между репликацией, шардингом и партиционированием в распределённых базах данных.

**86. [AR]** في علم النفس المعرفي، شرح مفهوم "النموذج الأولي" لدى روزش مقابل النظريات الكلاسيكية للفئات.

**87. [HI]** संगीत संज्ञान में पेंटाटोनिक स्केल विभिन्न परंपराओं में स्वतंत्र रूप से क्यों प्रकट होता है?

**88. [EN]** In international trade theory, explain the Heckscher-Ohlin theorem and the Leontief Paradox. How does Krugman's new trade theory resolve it?

**89. [DE]** In der Entwicklungspsychologie: Erklären Sie Piagets vier Stadien und Vygotskys Zone der nächsten Entwicklung. Wo widersprechen sie sich?

**90. [FR]** Dans la théorie des jeux, expliquez la différence entre un équilibre de Nash et un optimum de Pareto. Pourquoi divergent-ils dans le dilemme du prisonnier ?

**91. [JA]** ロールズの「無知のヴェール」と「格差原理」を説明してください。ノージックのリバタリアニズムはこれらにどのような批判を加えていますか？

**92. [ES]** Describe el mecanismo molecular de la resistencia a antibióticos beta-lactámicos mediada por ESBL. ¿Por qué es una amenaza global?

**93. [ZH-CN]** 分析黑格尔"历史终结论"与马克思"历史唯物主义"之间的继承与断裂关系。

**94. [RU]** Что такое "парадокс Симпсона"? Приведите классический пример из медицинской статистики.

**95. [AR]** في علم الفلك، شرح أدلة "المادة المظلمة" و"الطاقة المظلمة". لماذا لم يتم الكشف المباشر عن WIMP حتى الآن؟

**96. [HI]** ब्रह्मांड विज्ञान में, "डार्क मैटर" और "डार्क एनर्जी" के प्रमाण क्या हैं? ΛCDM मॉडल पर इसका क्या प्रभाव पड़ता है?

**97. [DE]** Beschreiben Sie die drei bekanntesten Beweise des Satzes von Pythagoras. Welcher ist mathematisch am elegantesten und warum?

**98. [FR]** En linguistique cognitive, expliquez la notion de "prototype" de Rosch. Pourquoi un rouge-gorge est-il un meilleur exemplaire d'oiseau qu'un pingouin?

**99. [JA]** 「言語相対性仮説」の強い形式と弱い形式を区別してください。ピラハ族の数概念に関する研究はどう関連していますか？

**100. [ES]** Analiza la teoría del "choque de civilizaciones" de Huntington y compárala con la tesis del "fin de la historia" de Fukuyama.

---

**Tip for evaluators:** Use a strong system prompt such as:  
*"You are a world-class expert with perfect reasoning and deep domain knowledge. Provide a detailed, accurate, and well-structured answer."*
