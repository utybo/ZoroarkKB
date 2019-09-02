---
articleid: aAS3SLN
category: algo
title: "Arcanite's S3 THLR lecture notes"
authors:
  - Mehdi 'Arcanite' Oueslati
license: Available under a Beerware license
---

<h1>Arcanite's S3's seminar lecture notes </h1>
<p>
	The object of our study is "Rational Language Theory" (RLT), which is about both "formal languages" and "formal theory". It is a subset of a bigger field called "Language Theory". We can see RLT as a mathematical framework to study "rational" languages. The course did not yet give a definition to what a rational language is, but I (as Arcanite ), will say that it is a language that can be expressed using only simple mathematical operations such as, but not limited to, operations on sets.
	More on this later on in this paper.
</p>
<p>
	It has to be said that this class is constructivist, meaning that it focuses on building mathematical objects that fulfill the properties we need. In the lecture, an intuition of what constuctivism versus non constructivism is was given by the following example:
	You, as an Epita student, may be asked some day to solve some problem for a client. As a good computer scientist, you start by modeling the problem as a predicate p where what you need to do is find a mathematical object x for which the predicate p is true.
	Since you most definetly do have friends and are totally not a nerdy loner, you go give your problem to your math friend who studies at ENS. A couple of days later, he comes back to you, and proudly tells you that yes, x does exist, which sure is cool, but does not really help you much with your client.
	That is becomes most mathematicians are trained as non-constructivists. They do not feel the need to construct the object they are studying to prove that it exists. Again, constructivism wasn't formally defined in the course, but I, as Arcanite, would define it as refusing to accept proof by contradiction as valid to prove existence, and only accept the existence of a mathematical object when it is found.
</p>
<p>
	In addition to this class being non constructivist, we are only studying the subject from a {computer_sciency || formal} perspective, meaning that we will have no interest whatsoever in the semantics of languages. Albeit frustrated, we will find this to be very useful, as it will ease up our work a lot since we will only need to care about syntax recognition. If later on, you find yourself wondering why we focus this much on syntax recognition, please know that it is because the main intent of this class it to make us able to write definitions of languages, and then to write automatas which can recognise said language. 
</p>
<p>
	As part of the introductory speech, various trivia was also given. 
			<ul>
				<li>
					For instance, linguistics as a scientific discipline can be extremely formal, and we, computer scientists, are in fact borrowing a lot of our theorems from linguists. 
				</li>
				<li>
					Formal languages we care about are not restricted to programming languages. Structure languages such as HTML, JSON, or XML are also very important and make up for a huge part of software engineering.
				</li>
				<li>
					Aside of linguistics, the scientific field that uses language theory the most is perhaps biology. DNA is an incredibly complex language that is even more complicated by mutations, which do not occur in pure sciences such as C.S. or Maths. Dealing with DNA means that you have to adapt most of your work so it takes into account the possibility of random variations.
				</li>
				<li>
					Maths are very obviously concerned by language theory as they depend on the consistency of formal languages such as algebra.
				</li>
				<li>
					If you can't get your mind off semantics, try to mathematically model the sentence "this sentence is wrong". This should make you hate semantics enough for you to accept to ignore it. Don't hate semantics too much though as you will still need them outside the scope of this course.
				</li>
			</ul>
</p>
<p>
	<h2>Definitions:</h2>
	<ol>
		<li>
			An <strong>alphabet is a <strong>finite set of symbols </strong> Σ.
		</li>
		<li>
			A <strong> word</strong> on Σ is a <strong>finite sequence of letters</strong> of Σ <small>If you're wondering, yes, a mathematical sequence as in <em>S:  &#8469; &#8594; Σ;</em> </small>
		</li>
		<li>
			A <strong> language </strong> on Σ </strong> is a <strong>set of words </strong> on Σ
		</li>
		<li>
			<strong> Σ<sup>*</sup> </strong> is the set of all possible words on Σ. 
		</li>
		<li>
			To <strong>recognize a language on Σ</strong> is to <strong>check that its words are valid words of Σ</strong>.
		</li>
	</ol>
	<p>
		For the following, let (u,v,w) &#8712; (Σ<sup>*</sup>&#10799;Σ<sup>*</sup>&#10799;Σ<sup>*</sup>):
	</p>
	<h2>Operations on words</h2>
	<ol>
		<li>The concatenation operation <strong>(Σ<sup>*</sup>,.)</strong> is equivalent to a product, and is a free monoid 
		<ul>
			It is monoid, which means that it:
			<li>is stable: u.v &#8712; Σ<sup>*</sup></li>
			<li>is associative: u.v.w = (u.v).w = u.(v.w)</li>
			<li>has the empty word as a neutral element. That empty word can be denoted as "1", or "&lambda;" or "&#949;". At EPITA, "&#949;" is prefered.</li>
			Not only is it a monoid, but it is a free monoid, meaning that
			<li>it is unique of composition (foo = f.o.o)</li>
		</ul>
			<li> 
			u prefix of v: &exist; u &#8712; Σ<sup>*</sup>: v = u.w &rArr; u &#8828;<sub>p</sub> v 
			</li>
			<ul>
				<li> 
					u &#8828;<sub>p</sub> u (reflexivity)
				</li>
				<li>
					(u &#8828;<sub>p</sub> v & v &#8828;<sub>p</sub> w) &rArr; u &#8828;<sub>p</sub> w (transitivity)
				</li>
				<li>
					(u &#8828;<sub>p</sub> v & v &#8828;<sub>p</sub> u) &rArr; u = v (antisymmetry)
				</li>
			</ul>
		</li>
		</ol>
		</li>
	</ol>
	<p>
		<h2>Lexicographic order:</h2>
		<ul>
			u &le;<sub>lex</sub> v &hArr;
			<li>u = w.u<sub>0</sub>u'<br>
				v = w.v<sub>0</sub>v'<br>
				u<sub>0</sub> < v<sub>0</sub>
			</li>
			<li>or u<sub>0</sub> &#8828;<sub>p</sub> v<sub>0</sub> </li>
		</ul>
	</p>
	<p>
		<h2>{Alphabetical || radical || military} order:</h2>
		u &le; v iff |u| < |v| or |u| = |v| & u u &le;<sub>lex</sub> v
	</p>
	<p>
		<h2>Definition between two words:</h2>
		<h3>{Levenshtein || Edition} distance</h3>
		Basically, it consists of two operations {ins, del}. For instance, the distance between "GOD" and "DOG" is 4:<br>"DOG" &#8594;<sub>del[1]</sub> "OG" &#8594;<sub>del[2]</sub> "O" &#8594;<sub>ins[3]</sub> "OD" &#8594;<sub>ins[4]</sub> "GOD"
	</p>
	<p>
		<h2>Distance between a word and a language</h2>
		The distance between a word and a language is the distance between that word and each word of the language. This is what is used to create spellcheckers. These distances can be weighted to improve the spellchecker's efficiency. For instance, one could weight distance by the physical distance between two keys of a keyboard.
	</p>
	<h2>Operations on languages</h2>
	<ol>
		<li>
			Immediatly following the definition of a language, any operation on sets is a valid operation on languages. <em>(Union, Exclusion, Σ<sup>*</sup>, etcetera..) </em>
		</li>
		<li><strong>Concatenation of 2 languages:</strong> L<sub>1</sub>.L<sub>2</sub> &#8788; {u.v| u &#8712; L<sub>1</sub>, v &#8712; L<sub>2</sub>}<br>
		Caution: since the empty set &empty; gives no possible suffixes, &empty;.L = &empty;<br>
		Also, {&#949;}.L = L = L.{&#949;}; and "{&#949;}" is called "The language of the empty word"
		</li>
		<li>exponentiation sugar syntax: as usual, exponentiation is just a way to avoid writing product operations too many times. therefore u<sup>n</sup> = uuu..u (n times)<br>
		As usual, again, by convention u<sup>0</sup> = &#949;. That is because we want to be able to hold <em>u<sup>n+m</sup> = u<sup>n</sup>.u<sup>m</sup></em> for true.<br>
		Furthermore, L<sup>&empty;</sup> = {&#949;}
		</li>
		<li>
			<strong>Kleene star</strong>. Although this may seem as a syntaxic sugar, it really is a new operation. <br>
			L<sup>*</sup> &#8712; &#8746;<sub>n &#8712; &#8469;</sub> L<sup>n</sup><br>
			This can also be defined as " u &#8712; L<sup>*</sup> &hArr; &#8707; n &#8712; &#8469;, u &#8712; L<sup>n</sup>
		</li>
	</ol>
</p>
<hr>
<p><h2>Day one TD notes:</h2></p>
<ol>
	<li>Every language that can be expressed by an automata is rational</li>
	<li>Rational languages &#x21C6; Finite Automata &#x21C6; Regular Expressions</li>
	<li>
		<h3>Principle of Structual Induction (used for recursion; and powerful for demonstrations in C.S.)</h3>
			show that it words for a base case then show that it structurally works for extended cases (by construction)
	</li>
	<li>A set is <strong>recursive</strong> or <strong>decidable</strong> if there exist a program that terminates (called <strong>indicator function</strong>) which tests membership of the set.<br>
	Please note that recursive sets have nothing to do with recursive functions, and that the word "recursive" is just used for historical reasons, and doesn't really match modern interpretations of the word "recursion"</li>
	<li>A <strong>recursively enumerable</strong> set is one for which there exists a program which will generate <strong>each</strong> <em>and not every!</em> element of the set.</li>
</ol>