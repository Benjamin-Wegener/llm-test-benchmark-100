多语言问题解答
1. Git rebase 错误 (德语)
git rebase -i HEAD~15000 会失败，因为仓库只有10,000个提交。这是经典错误，因为rebase试图重写比实际存在更多的历史记录，导致无法找到这些提交。

2. Python 相等性与同一性 (英语)
[] == [] 为True因为值相等，[] is [] 为False因为它们是不同对象。CPython中，空列表每次创建新对象，引用计数分别为1，但is检查的是内存地址而非值。

3. CAP定理 (法语)
强一致性保证所有节点同时看到相同数据，最终一致性允许临时不一致但保证最终收敛。DynamoDB选择高可用性和分区容忍性，牺牲强一致性，采用最终一致性模型。

4. LRU缓存装饰器 (日语)
python
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)

# 未缓存: O(2^n), 缓存后: O(n)
5. 向量时钟 (西班牙语)
python
class VectorClock:
    def __init__(self, node_id):
        self.clock = {node_id: 0}
    
    def increment(self):
        self.clock[node_id] += 1
    
    def update(self, received_clock):
        for node, time in received_clock.items():
            self.clock[node] = max(self.clock.get(node, 0), time)
6. 量子纠缠 (中文)
量子纠缠展现非局域关联，测量一方瞬间影响另一方，违反贝尔不等式证明量子力学不能用局域隐变量理论解释，体现量子系统的整体性。

7. SQL中位数 (俄语)
sql
SELECT department_id, 
       PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary) 
       OVER (PARTITION BY department_id) as median_salary
FROM employees
8. 博弈论 (阿拉伯语)
纳什均衡是无人能通过单方面改变策略获益的状态，帕累托最优是无法在不损害他人情况下改善任何人处境。囚徒困境中纳什均衡(背叛,背叛)不是帕累托最优，因为(合作,合作)能让双方更好。

9. 发展心理学 (印地语)
पियाजे के 4 चरण: संवेदी-प्रेरक, पूर्व-संक्रियात्मक, ठोस संक्रियात्मक, और औपचारिक संक्रियात्मक। वायगोत्स्की का ZPD वह क्षेत्र है जहां बच्चा सहायता से सीख सकता है। मुख्य विरोध: पियाजे का चरण-आधारित दृष्टिकोण बनाम वायगोत्स्की का सामाजिक-सांस्कृतिक दृष्टिकोण।

10. Rust Singleton (英语)
rust
use std::sync::{Once, OnceLock};

struct Singleton {
    data: i32,
}

impl Singleton {
    fn new() -> Self {
        Singleton { data: 0 }
    }
    
    fn get_instance() -> &'static Singleton {
        static INSTANCE: OnceLock<Singleton> = OnceLock::new();
        INSTANCE.get_or_init(|| Singleton::new())
    }
}
lazy_static和OnceLock避免手动加锁的复杂性和性能开销。

11. 分布式系统故障 (德语)
Byzantine Fault指节点可能任意行为(包括恶意)，Crash Fault指节点只是停止工作。PBFT需要n >= 3f + 1因为必须容忍f个恶意节点并保证诚实节点占多数(2f + 1 > f)。

12. 微服务事务 (法语)
Saga编排式需要中央协调器， choreography式通过事件驱动。Two-Phase Commit提供强一致性但阻塞且不适合微服务。Saga适合长时间运行的业务事务，提供最终一致性。

13. Rust所有权 (日语)
所有権システムはコンパイル時に各値が唯一の所有者を持つことを保証します。借用は参照を作成し、ライフタイムは参照が有効な期間を追跡します。二重解放は所有権移動により防止され、ムーブ後の使用はコンパイルエラーになります。

14. 费米悖论 (西班牙语)
La paradoja de Fermi pregunta por qué no vemos evidencia de civilizaciones extraterrestres. La ecuación de Drake estima el número de civilizaciones detectables. El "Gran Filtro" sugiere que alguna barrera evolutiva o tecnológica impide que las civilizaciones se vuelvan detectables o sobrevivan.

15. 梯度问题 (中文)
RNN中梯度消失/爆炸是因为反向传播时连续矩阵乘法导致特征值指数级衰减/增长。LSTM/GRU通过门控机制和梯度高速公路缓解此问题，允许梯度更稳定地流动。

16. Raft算法 (俄语)
Raft选举领导者时使用随机超时避免分裂投票。当候选者获得多数票(n/2 + 1)时成为领导者。随机超时确保不会多个候选者同时发起选举，减少split-brain场景。

17. 五声音阶 (阿拉伯语)
السلم الخماسي يظهر بشكل مستقل في تقاليد موسيقية غير مرتبطة بسبب قيود نفسي-صوتية عالمية ومنطق هيكلي للموسيقى، وليس مجرد مصادفة ثقافية.

18. 音乐认知 (印地语)
पेंटाटोनिक स्केल असंबंधित संगीतिक परंपराओं में स्वतंत्र रूप से प्रकट होता है क्योंकि यह मानव श्रवण प्रणाली की मूलभूत विशेषताओं और ध्वनि के भौतिक गुणों को दर्शाता है, जो सार्वभौमिक मनो-ध्वनिक बाधा का प्रमाण है।

19. 普罗提诺与亚里士多德 (德语)
Plotins Henologie widerlegt Aristoteles' Substanzbegriff nicht vollständig. Während Aristoteles Substanz als individuelle Entität sieht, betont Plotin das Eine als Quelle aller Existenz. Nous und Psyche sind Emanationen des Einen, nicht unabhängige Substanzen.

20. 熵的统计力学 (法语)
Pour un gaz parfait monoatomique, S = k ln(Ω) où Ω est le nombre de microétats. En thermodynamique, dS = δQ/T. Les deux sont cohérents car Ω ∝ V(T)^(3N/2) pour un gaz parfait, et δQ = (3/2)Nk dT, ce qui donne la même expression pour dS.

21. 康德的物自体 (日语)
カントの「物自体」と現象の区別は、認識論的に一貫している。ヤコービはカントが現象と実在を完全に分離しすぎていると批判し、フィヒテは「私」という絶対的な出発点から両者を統合しようとした。

22. 停机问题 (西班牙语)
El problema de detención es indecidible por reducción diagonal: suponer que existe un programa H que decide si cualquier programa P se detiene con entrada X lleva a contradicción al construir un programa D que se detiene si H dice que no se detiene, y viceversa. Esto implica que la equivalencia de programas también es indecidible.

23. 前景理论 (中文)
前景理论认为人们对损失比收益更敏感(损失厌恶)，且决策依赖于参考点。爱荷华赌博任务显示，正常人会避开高损失风险的牌组，而脑损伤患者无法学习这种风险规避，说明情绪在决策中的作用。

24. 量子测量悖论 (俄语)
Коллапс волновой функции противоречит унитарной эволюции уравнения Шрёдингера, которое детерминистично и обратимо. Многомировая интерпретация решает это, утверждая, что коллапса нет - все возможные исходы реализуются в разных ветвях вселенной.

25. 形式语法 (阿拉伯语)
القواعد السياقية الحرة تولد لغات يمكن التعرف عليها بواسطة المكدس، بينما القواعد السياقية الحساسة أكثر تعقيداً. تسلسلية تشومسكي غير كافية للغات الطبيعية لأنها لا تستطيع وصف التبعيات عبر المسافات الطويلة والظواهر الدلالية.

26. 形式语言 (印地语)
संदर्भ-मुक्त व्याकरण और हल्के संदर्भ-संवेदनशील व्याकरण के बीच मुख्य अंतर यह है कि संदर्भ-मुक्त व्याकरण में उत्पादन नियम संदर्भ-स्वतंत्र होते हैं। चॉम्स्की पदानुक्रम प्राकृतिक भाषा वाक्यविन्यास का वर्णन करने के लिए अपर्याप्त है क्योंकि प्राकृतिक भाषाओं में संदर्भ-संवेदनशीलता और अर्थ-आधारित संरचनाएं होती हैं।

27. 宪法原则 (英语)
Non-delegation doctrine限制国会将立法权委托给行政机构，Chevron deference要求法院尊重行政机构对模糊法律的合理解释。Loper Bright Enterprises v. Raimondo (2024)推翻Chevron，恢复了司法部门对法律解释的最终权威，限制了行政裁量权。

28. 射影几何 (德语)
Projektiv-geometrisch unterscheiden sich Ellipse und Parabel durch ihre Schnittpunkte mit der Ferngeraden: Eine Ellipse schneidet die Ferngeraden nicht (imaginäre Schnittpunkte), während eine Parabel die Ferngeraden in einem Punkt berührt (doppelter Schnittpunkt).

29. 黑格尔的承认 (法语)
La "reconnaissance" chez Hegel est le processus par lequel la conscience de soi se réalise à travers l'autre. La dialectique maître-esclave montre que la reconnaissance mutuelle est nécessaire pour la liberté, préfigurant la théorie d'Axel Honneth sur la reconnaissance sociale comme fondement de l'identité.

30. 菲利普斯曲线 (日语)
フィリップス曲線のトレードオフは、インフレと失業率の逆相関を示していましたが、1970年代のスタグフレーション(高インフレと高失業の同時発生)によって崩壊しました。これは供給ショックと期待インフレの上昇によるものです。

31. 汉密尔顿规则 (西班牙语)
La regla rB > C de Hamilton establece que un gen altruista se propaga si el beneficio (B) para el receptor multiplicado por el grado de parentesco (r) excede el costo (C) para el donante. En himenópteros eusociales, las hermanas comparten 75% de genes (r=0.75), favoreciendo la evolución de la esterilidad obrera.

32. 哥德尔定理 (中文)
第一定理: 任何包含皮亚诺算术的形式系统都存在不可判定的真命题。第二定理: 这样的系统无法证明自身的一致性。这终结了希尔伯特计划，即用有限方法证明数学的完备性和一致性。

33. 归纳问题 (俄语)
Проблема индукции Юма: нет логического обоснования для обобщения от наблюдений к законам. Поппер предложил фальсификационизм: научные теории не верифицируемы, а только фальсифицируемы - они должны делать предсказания, которые можно опровергнуть.

34. 原型理论 (阿拉伯语)
نموذج روزش الأولي يعتبر أن الفئات لها أعضاء نموذجيين (مثل الطائر الأحمر) وأعضاء هامشيين (مثل البطريق). النظريات الكلاسيكية تتطلب معايير ضرورية وكافية، بينما النظريات الحديثة تعترف بالتدرج والسياقية في التصنيف.

35. 认知语言学 (印地语)
रोश के "प्रोटोटाइप" की अवधारणा के अनुसार, श्रेणियों के सदस्य समान नहीं होते हैं - कुछ अधिक प्रतिनिधि होते हैं (जैसे लाल रोबिन पक्षी के लिए), जबकि शास्त्रीय सिद्धांत सभी सदस्यों को समान मानते हैं। यह मानव वर्गीकरण की वास्तविक प्रकृति को बेहतर दर्शाता है।

36. 基因编辑 (德语)
CRISPR-Cas9 schneidet DNA an spezifischen Stellen, kann aber Off-Target-Effekte haben. Base Editing modifiziert einzelne Basen ohne Doppelstrangbruch, ist präziser und sicherer, da es keine DNA-Brüche verursacht, die zu unerwünschten Mutationen führen können.

37. 因果图 (法语)
La distinction entre corrélation et causalité nécessite le cadre des DAG (Directed Acyclic Graphs) de Judea Pearl. Un "collider" est une variable influencée par deux autres variables; conditionner sur un collider crée une corrélation artificielle entre ses causes (biais de sélection).

38. 奥斯曼米利特 (土耳其语)
Osmanlı'nın "Millet sistemi" dini çoğulculuğun bir örneği olarak görülebilir çünkü farklı dini topluluklara özerklik tanıyordu. Ancak aynı zamanda yapısal eşitsizliğin kurumsallaşmasıydı çünkü Müslüman olmayanlar ikinci sınıf vatandaş statüsündeydi ve vergi ayrımcılığına maruz kalıyordu.

39. 修昔底德陷阱 (西班牙语)
La "trampa de Tucídides" sugiere que cuando una potencia emergente (China) amenaza a una establecida (EE.UU.), la guerra es probable. Sin embargo, como modelo predictivo tiene limitaciones: ignora la interdependencia económica, las instituciones internacionales y las diferencias ideológicas internas.

40. R2P原则 (中文)
"保护责任"原则主张主权国家有责任保护其人民免受大规模暴行，若失败则国际社会可干预。2011年利比亚干预展示了R2P的张力：安理会授权保护平民，但北约行动超出授权导致政权更迭，引发关于R2P被滥用的争议。

41. 哥德尔定理适用范围 (俄语)
Теорема Геделя применима к системам, содержащим арифметику Пеано, потому что они достаточно богаты для самореференции. Теория полных упорядоченных полей (как вещественные числа) не содержит арифметику натуральных чисел в достаточной степени для построения неразрешимых утверждений.

42. 发展心理学 (阿拉伯语)
تتكون نظرية بياجيه من أربع مراحل: الحسية الحركية، ما قبل العمليات، العمليات العينية، والعمليات الصورية. مفهوم فيغوتسكي لـ "منطقة النمو القريب" يركز على التعلم الاجتماعي. يتعارضان في أن بياجيه يرى التطور فردياً بينما يراه فيغوتسكي اجتماعياً.

43. 奥斯曼改革 (土耳其语)
Osmanlı İmparatorluğu'nun "Millet sistemi" hem dini çoğulculuğun başarılı bir örneği hem de yapısal eşitsizliğin kurumsallaşması olarak görülebilir. Tanzimat reformları (1839-1876) ile millet sistemi modernleştirilmeye çalışıldı ancak milliyetçilik akımları ve Avrupa müdahaleleri sistemi zayıflattı.

44. 统计假设检验 (英语)
Type I error: rejecting true null hypothesis (false positive). Type II error: failing to reject false null hypothesis (false negative). These don't map cleanly to Bayesian inference which uses posterior probabilities. Replication crisis refers to the difficulty of reproducing many scientific studies, especially in psychology and medicine.

45. 调和级数 (德语)
Die harmonische Reihe Σ(1/n) divergiert, obwohl 1/n → 0. Oresme's Beweis: Gruppiere Terme: 1 + 1/2 + (1/3+1/4) + (1/5+...+1/8) + ... > 1 + 1/2 + 1/2 + 1/2 + ... → ∞.

46. 国际法中的决议 (法语)
Une résolution de l'Assemblée générale de l'ONU n'a pas de force juridique contraignante (contrairement aux traités), mais a une portée politique et morale. Le jus cogens désigne les normes impératives du droit international (interdiction du génocide, torture, esclavage) qui ne peuvent être dérogées par aucun État.

47. 电车难题 (日语)
「トロッコ問題」と「歩道橋問題」の直観のズレは、功利主義と義務論だけでなく、グリーンの二重過程理論で説明できる。歩道橋問題では個人的な暴力への情動的反応が強く働き、道徳的判断を阻害する。

48. 缪勒-莱尔错觉 (西班牙语)
La ilusión de Müller-Lyer persiste porque nuestro sistema visual interpreta las líneas en términos de profundidad y perspectiva, no solo como longitudes absolutas. Las puntas de flecha activan procesos perceptuales automáticos que son difíciles de suprimir incluso con conocimiento consciente.

49. 弦理论紧化 (中文)
紧化是将额外维度卷曲成微小空间的过程。额外维度必须紧化因为我们只观测到4维时空。卡拉比-丘流形提供了保持超对称性的紧化方案，其特殊几何性质决定了粒子物理的低能有效理论。

50. HIV治疗 (俄语)
Ингибиторы обратной транскриптазы блокируют превращение РНК ВИЧ в ДНК. Монотерапия приводит к резистентности, потому что вирус быстро мутирует. Комбинированная антиретровирусная терапия (ART) использует несколько препаратов одновременно, снижая вероятность одновременной мутации ко всем препаратам.

51. 哈耶克知识问题 (阿拉伯语)
مشكلة هايك المعرفية تقول إن المعلومات الاقتصادية مشتتة ولا يمكن تجميعها مركزياً. التخطيط الاقتصادي المركزي لا يستطيع استبدال إشارات الأسعار لأن الأسعار تحمل معلومات لا يمكن استخلاصها من البيانات الإحصائية وحدها. هذا النقاش لا يزال صالحاً ضد التخطيط بالذكاء الاصطناعي لأن المعرفة الضمنية والمحلية لا يمكن رقمنتها بالكامل.

52. 哈耶克知识问题 (印地语)
हायेक की ज्ञान समस्या के अनुसार, आर्थिक जानकारी विकेंद्रित और स्थानीय होती है, जिसे केंद्रीय नियोजक संकलित नहीं कर सकता। केंद्रीय नियोजित अर्थव्यवस्था वितरित मूल्य संकेतों का विकल्प नहीं बन सकती क्योंकि मूल्य तंत्र लाखों व्यक्तियों की व्यक्तिगत प्राथमिकताओं और परिस्थितियों को संप्रेषित करता है।

53. 恒星演化 (英语)
25太阳质量恒星经历: H燃烧(主序)→ He燃烧(红超巨星)→ C, Ne, O, Si燃烧(洋葱结构)→ Fe核形成。铁标志终点因为核聚变到铁不再释放能量(铁有最高结合能)，反而吸收能量，导致核心坍缩和超新星爆发。

54. 家族相似性 (德语)
Wittgensteins "Familienähnlichkeit" zeigt, dass Begriffe nicht durch notwendige und hinreichende Bedingungen definiert sind, sondern durch überlappende Ähnlichkeiten wie bei Familienmitgliedern. Dies widerlegt die klassische Begriffstheorie, dass alle Beispiele eines Begriffs gemeinsame Merkmale haben müssen.

55. 表观遗传学 (法语)
Les modifications d'histones (acétylation, méthylation) et la méthylation de l'ADN sont des marques épigénétiques qui régulent l'expression génique sans changer la séquence d'ADN. Elles sont héritables car les enzymes de maintenance copient ces marques lors de la réplication de l'ADN.

56. 哈耶克知识问题 (日语)
ハイエクの知識問題とは、経済に必要な情報は分散的であり、中央計画経済は価格シグナルを代替できないという主張です。価格メカニズムは何百万人もの個人の知識と選好を集約する唯一の効率的な方法です。

57. 技术债务 (西班牙语)
La "deuda técnica" es la acumulación de soluciones subóptimas en el desarrollo de software. Los sistemas legados ilustran la trampa irreversible porque el costo de refactorización aumenta exponencialmente con el tiempo, mientras que el valor del código disminuye, creando un ciclo vicioso de parches temporales.

58. 全景监狱 (中文)
福柯的"全景监狱"概念描述了现代监控社会，其中被监视者内化监视者的目光，实现自我规训。当代数字监控通过大数据和算法扩展了这一概念，但其局限性在于忽视了被监控者的能动性和反抗可能性。

59. Transformer (俄语)
Трансформеры используют механизм внимания с позиционным кодированием, который позволяет обрабатывать все токены параллельно и улавливать зависимости на любых расстояниях. Это превосходит рекуррентные сети (RNN), которые страдают от проблемы затухания градиента и последовательной обработки.

60. 宪法审查 (阿拉伯语)
تستخدم فرنسا الرقابة الدستورية المسبقة (قبل إصدار القوانين) بدلاً من المراجعة القضائية الأمريكية (بعد الإصدار). النموذج الفرنسي يمنع انتهاك الدستور مسبقاً لكنه يقلل من دور القضاء، بينما النموذج الأمريكي يعطي القضاة سلطة أكبر لكنه قد يؤدي إلى تأخير تشريعي.

61. 比较宪法 (印地语)
तुलनात्मक संवैधानिक कानून में, फ्रांस पूर्व-नियंत्रण संवैधानिकता परीक्षण का उपयोग करता है जहां संवैधानिक परिषद कानूनों को उनके लागू होने से पहले जांचती है। इसके विपरीत, अमेरिकी मॉडल में न्यायपालिका कानूनों को बाद में चुनौती दे सकती है। फ्रांसीसी मॉडल संसदीय संप्रभुता को सीमित करता है जबकि अमेरिकी मॉडल न्यायिक सक्रियता को बढ़ावा देता है।

62. 量子化学 (德语)
Hartree-Fock unterschätzt die Korrelationsenergie, weil es Elektronen als unabhängig in einem mittleren Feld behandelt und die instantanen Elektron-Elektron-Korrelationen vernachlässigt. Post-HF-Methoden wie CI (Configuration Interaction), CC (Coupled Cluster) und DFT (Density Functional Theory) berücksichtigen diese Korrelationen.

63. 中世纪历史 (法语)
La querelle des investitures (XIe-XIIe siècle) opposait le pape et l'empereur sur le droit de nommer les évêques. Elle transforma la relation pouvoir temporel/spirituel en affirmant l'indépendance partielle de l'Église. Le Concordat de Worms (1122) établit un compromis: l'empereur investit les évêques de leurs droits temporels, le pape de leurs droits spirituels.

64. 麦克斯韦妖 (日语)
熱力学第二法則は孤立系でエントロピーが増大すると述べる。マクスウェルの悪魔は分子を選別してエントロピーを減少させる思考実験。ランダウアーの原理は、情報の消去にエネルギーが必要(kT ln 2)ことを示し、悪魔が情報を処理する際のエントロピー増大が第二法則を救う。

65. 言语行为 (西班牙语)
Austin y Searle distinguen: acto locucionario (producir enunciado), ilocucionario (intención comunicativa como prometer u ordenar), y perlocucionario (efecto en el oyente como persuadir). La fuerza ilocucionaria es la intención convencional detrás del acto.

66. 流动性陷阱 (中文)
流动性陷阱指名义利率降至零附近时，货币政策失效，因为人们偏好持有现金而非债券。日本"失去的二十年"显示，即使利率接近零，经济仍可能停滞，需要财政政策配合。

67. P vs NP (俄语)
P: задачи, решаемые за полиномиальное время. NP: задачи, решения которых проверяемы за полиномиальное время. NP-полные задачи - самые сложные в NP. Задача коммивояжёра: найти кратчайший путь через n городов. Если P≠NP (вероятно), то NP-полные задачи не имеют эффективных алгоритмов решения.

68. 历史终结与历史唯物主义 (阿拉伯语)
هيغل رأى أن التاريخ ينتهي عندما يتحقق الوعي الكامل بالحرية في الدولة المثالية. ماركس حول هذا إلى مادية تاريخية، حيث التاريخ ينتهي بالشيوعية بدلاً من الدولة البروسية. كوجيف قرأ هيغل من خلال عدسة الصراع بين السيد والعبد، مما أثر على الفلسفة السياسية في القرن العشرين بدمج الوجودية مع الماركسية.

69. 黑格尔与马克思 (印地语)
हेगेल की "इतिहास की समाप्ति" और मार्क्स की "ऐतिहासिक भौतिकवाद" के बीच संबंध जटिल है। हेगेल ने इतिहास को आत्मा की स्वतंत्रता की प्रगति के रूप में देखा, जबकि मार्क्स ने इसे भौतिक उत्पादन संबंधों के विकास के रूप में पुनर्व्याख्या की। दोनों में ऐतिहासिक विकास की दिशात्मकता का विचार है, लेकिन मार्क्स ने हेगेल के आदर्शवाद को भौतिकवाद में उलट दिया।

70. 音乐理论 (英语)
Equal-tempered tritone (augmented 4th/diminished 5th) creates tension because it's equidistant from perfect intervals, but resolves in dominant seventh chords because the tritone (3rd and 7th) creates instability that resolves to stable intervals (3rd to tonic, 7th to 3rd).

71. 债务通缩 (德语)
Deflation erhöht den realen Wert von Schulden. Schuldner müssen mehr in realen Werten zurückzahlen, was zu Ausgabenkürzungen führt. Dies senkt die Preise weiter (Schulden-Deflations-Spirale nach Fisher 1933), was die Wirtschaft in eine Rezession treibt.

72. 象征暴力 (法语)
Bourdieu's "violence symbolique" est l'imposition de catégories de perception et d'appréciation par la force sociale. L'habitus, en tant que système de dispositions incorporées, reproduit les inégalités en faisant paraître naturelles des hiérarchies sociales arbitraires.

73. 实用主义 (日语)
パース: 真理は探究の最終的合意点。ジェームズ: 真理は実用的価値と"現金価値"。デューイ: 真理は問題解決の道具で、経験を通じて検証される。三者の違いは真理の基準にある: パースは理想的合意、ジェームズは個人的満足、デューイは社会的実践。

74. 意识难题 (西班牙语)
El "problema difícil de la conciencia" de Chalmers pregunta por qué los procesos físicos producen experiencia subjetiva (qualia). Los "zombis filosóficos" (seres físicamente idénticos pero sin conciencia) desafían al fisicalismo porque muestran que la conciencia no se deduce lógicamente de los hechos físicos.

75. 图灵测试 (中文)
图灵测试的哲学预设是行为主义:如果机器表现得像人，它就有智能。塞尔的"中文房间"反驳:语法操作不等于语义理解，即使通过测试也不证明真正的理解或意识。

76. CRISPR机制 (俄语)
CRISPR-Cas9: гРНК направляет Cas9 к целевой ДНК. Домен PAM (Protospacer Adjacent Motif) необходим для распознавания. Нуклеазные домены RuvC и HNH разрезают комплементарную и некомплементарную цепи ДНК соответственно, создавая двуцепочечный разрыв.

77. 汉密尔顿规则 (阿拉伯语)
قاعدة rB > C لهاملتون تفسر الإيثار الظاهري في التطور: إذا كانت الفائدة (B) للمستفيد مضروبة في درجة القرابة (r) أكبر من التكلفة (C) للمانح، فإن الجين الإيثاري ينتشر. في الحشرات الاجتماعية (مثل النحل)، تكون الأخوات متشابهات بنسبة 75%، مما يفسر تطور العقم لدى العاملات.

78. 发展心理学比较 (印地语)
पियाजे का सिद्धांत बालक के संज्ञानात्मक विकास को चरणों में विभाजित करता है (संवेदी-प्रेरक, पूर्व-संक्रियात्मक, ठोस संक्रियात्मक, और औपचारिक संक्रियात्मक), जबकि वायगोत्स्की सामाजिक-सांस्कृतिक संदर्भ पर जोर देते हैं और "निकटस्थ विकास का क्षेत्र" की अवधारणा प्रस्तुत करते हैं। वे मौलिक रूप से इस बात पर विरोध करते हैं कि पियाजे विकास को व्यक्तिगत खोज मानते हैं, जबकि वायगोत्स्की इसे सामाजिक अंतःक्रिया का परिणाम मानते हैं।

79. Go并发代码 (英语)
The code has a race condition because Get() is called outside the lock, then mu.Lock() is acquired, but another goroutine could modify the data between Get() and the lock acquisition. With 1000 goroutines, this causes lost updates and incorrect increments. Fix: move Get() inside the critical section or use atomic operations.

80. GPCR信号传导 (德语)
GPCRs sind 7-Transmembran-Rezeptoren, die G-Proteine aktivieren. Ligandenbindung induziert Konformationsänderung, Gα-Untereinheit tauscht GDP gegen GTP und dissoziiert. Gα und Gβγ aktivieren Effektoren (Adenylylcyclase, Phospholipase C). Sie sind häufigste Ziele, weil sie an vielen Prozessen beteiligt sind und extrazellulär zugänglich.

81. 默认模式网络 (法语)
Le Default Mode Network (DMN) comprend le cortex cingulaire postérieur, le cortex préfrontal médian et les lobes pariétaux inférieurs. Il s'active au repos et lors de la rumination, de la mémoire autobiographique et de la théorie de l'esprit. Il est crucial pour la conscience de soi et la simulation mentale.

82. 阿罗不可能性定理 (中文)
三个公理:1)无限制定义域(所有偏好排序都可能),2)帕累托效率(如果所有人偏好A>B，则社会偏好A>B),3)无关独立性(社会对A和B的排序只取决于个人对A和B的排序)。阿罗证明:当选项>2时，不存在同时满足三个公理的社会选择函数。

83. 忒修斯之船 (西班牙语)
Locke: identidad en continuidad de memoria/conciencia. Hume: identidad es ficción, solo hay percepciones sucesivas. Parfit: identidad personal no es lo que importa, sino continuidad psicológica (Relation R). La nave de Teseo muestra que identidad depende de convenciones y grados de continuidad.

84. 多重实现性 (中文)
普特南的功能主义论证:同一心理状态可由不同物理状态实现(如疼痛可由人脑、章鱼脑或硅基实现)，因此心理学不能还原为物理学。这反对了心理-物理同一论，支持功能主义:心理状态由其功能角色定义，而非物理构成。

85. 分布式数据库 (俄语)
Репликация: копирование данных на несколько узлов для отказоустойчивости. Шардинг: горизонтальное разделение данных по ключу для масштабируемости. Партиционирование: разделение данных на части (может быть вертикальным или горизонтальным).

86. 原型理论 (阿拉伯语)
في علم النفس المعرفي، "النموذج الأولي" لروش يعني أن الفئات لها أعضاء نموذجيين (مثل العصفور كنموذج للطائر) وأعضاء أقل نموذجية (مثل البطريق). هذا يختلف عن النظريات الكلاسيكية التي تتطلب معايير محددة وضرورية لكل فئة.

87. 五声音阶 (印地语)
संगीत संज्ञान में पेंटाटोनिक स्केल विभिन्न परंपराओं में स्वतंत्र रूप से प्रकट होता है क्योंकि यह मानव श्रवण प्रणाली की मूलभूत विशेषताओं को दर्शाता है - विशेष रूप से consonant अंतरालों की प्राथमिकता और सांस्कृतिक सार्वभौमिकता के कारण।

88. 国际贸易理论 (英语)
Heckscher-Ohlin theorem: countries export goods that use their abundant factors intensively. Leontief Paradox: US (capital-abundant) exported labor-intensive goods, contradicting the theorem. Krugman's new trade theory resolves this with economies of scale, product differentiation, and monopolistic competition.

89. 发展心理学 (德语)
Piagets Stadien: 1) sensorisch-motorisch (0-2), 2) präoperational (2-7), 3) konkret-operational (7-11), 4) formal-operational (ab 11). Vygotskys ZPD: Zone zwischen aktuellem und potentiellem Entwicklungsniveau mit Hilfe. Widerspruch: Piaget sieht Entwicklung als stufenweise und individuell, Vygotsky als kontinuierlich und sozial vermittelt.

90. 博弈论 (法语)
Équilibre de Nash: personne ne peut améliorer son gain en changeant unilatéralement de stratégie. Optimum de Pareto: impossible d'améliorer le sort d'un joueur sans détériorer celui d'un autre. Dans le dilemme du prisonnier, l'équilibre (trahir, trahir) n'est pas Pareto-optimal car (coopérer, coopérer) serait meilleur pour les deux.

91. 罗尔斯与诺齐克 (日语)
ロールズの「無知のヴェール」: 公正な社会原理を選ぶための思考実験で、自分の地位を知らない状態で合意を形成する。「格差原理」: 不平等は最も恵まれない人の利益になる場合のみ許容される。ノージックのリバタリアニズム: 個人の権利と財産権を絶対視し、再分配を「強制労働」と批判。

92. ESBL耐药性 (西班牙语)
Las ESBL (β-lactamasas de espectro extendido) hidrolizan antibióticos β-lactámicos. La resistencia se media por plásmidos que codifican ESBL y se transfieren horizontalmente. Es una amenaza global porque limita opciones terapéuticas y aumenta mortalidad en infecciones por Enterobacteriaceae.

93. 黑格尔与马克思 (中文)
黑格尔"历史终结论"认为历史是绝对精神自我实现的过程，终点是自由意识的完全实现。马克思继承了辩证法但将其唯物主义化，认为历史动力是生产力与生产关系的矛盾，终点是共产主义。两者的继承关系在于辩证法和历史阶段性，断裂在于唯物主义对唯心主义的颠覆。

94. 辛普森悖论 (俄语)
Парадокс Симпсона: тенденция в группах меняется или исчезает при объединении. Пример: в исследовании лекарства, оно эффективнее для мужчин и женщин отдельно, но менее эффективно в целом, если группы имеют разный размер и базовый риск.

95. 暗物质与暗能量 (阿拉伯语)
أدلة "المادة المظلمة": دوران المجرات، عدسات الجاذبية، وتوزيع المادة في الكون المبكر. "الطاقة المظلمة": تسارع توسع الكون. لم يتم الكشف المباشر عن WIMP حتى الآن لأن تفاعلاتها ضعيفة جداً. نموذج ΛCDM يفترض أن الكون يتكون من ~5% مادة عادية، ~27% مادة مظلمة، و~68% طاقة مظلمة.

96. 宇宙学 (印地语)
ब्रह्मांड विज्ञान में, "डार्क मैटर" के प्रमाण गैलेक्सी घूर्णन वक्र और गुरुत्वाकर्षण लेंसिंग से मिलते हैं। "डार्क एनर्जी" ब्रह्मांड के त्वरित विस्तार का कारण है। ΛCDM मॉडल के अनुसार, ब्रह्मांड का ~68% डार्क एनर्जी, ~27% डार्क मैटर, और केवल ~5% सामान्य पदार्थ है।

97. 毕达哥拉斯定理证明 (德语)
Drei Beweise: 1) Euklids geometrischer Beweis (Flächenzerlegung), 2) Algebraischer Beweis (a²+b²=c² durch Quadrieren), 3) Ähnlichkeitsbeweis (Dreiecksähnlichkeit). Der eleganteste ist Euklids Beweis, weil er rein geometrisch ist, keine Algebra benötigt und die tiefe Struktur zeigt.

98. 认知语言学原型 (法语)
En linguistique cognitive, le "prototype" de Rosch désigne le membre le plus représentatif d'une catégorie. Un rouge-gorge est un meilleur prototype d'oiseau qu'un pingouin car il possède toutes les caractéristiques typiques (vol, chant, petite taille), alors que le pingouin en manque plusieurs.

99. 语言相对论 (日语)
強い形式: 言語が思考を決定する(サピア-ウォーフ仮説)。弱い形式: 言語が思考に影響を与える。ピラハ族の数概念研究: ピラハ語には正確な数詞がなく、彼らは正確な数量判断が困難で、弱い形式を支持する。

100. 文明冲突论 (西班牙语)
Huntington: tras la Guerra Fría, los conflictos serán culturales/civilizatorios, no ideológicos. Fukuyama: la historia termina con la democracia liberal. Huntington critica el universalismo occidental y predice choques entre civilizaciones (occidental, islámica, china, etc.). Fukuyama ve el fin de la historia ideológica.
