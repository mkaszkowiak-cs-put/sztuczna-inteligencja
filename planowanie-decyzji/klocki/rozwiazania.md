```
(define
	(domain world-of-blocks)
	(:requirements :adl)
	(:predicates
		(stoi-na ?powyzej ?ponizej)
		(podloga ?klocek)
		(mozna-podniesc ?klocek)
        (lapa-robota ?klocek)
	)

    ; 1 argument - paczka do podniesienia 
    (:action podnies-z-paczki 
        :parameters (?powyzej ?ponizej)
        :precondition
        (and
            ; nie istnieje paczka w lapie robota 
            (not (exists (?d) (lapa-robota ?d)))
            ; podnoszona paczka nie lezy na podlodze 
            (not (podloga ?powyzej))
            ; podnoszona paczka lezy na tej ponizej
            (stoi-na ?powyzej ?ponizej)
            ; pozna podniesc paczke powyzej
            (mozna-podniesc ?powyzej)
        )
        :effect 
        (and
            ; paczka trafia do lapy robota
            (lapa-robota ?powyzej)
            ; mozna podniesc ta ponizej 
            (mozna-podniesc ?ponizej)
            ; nie mozna podniesc tej w lapie robota 
            (not (mozna-podniesc ?powyzej))
            ; paczka w lapie juz nie stoi na ponizszej  
            (not (stoi-na ?powyzej ?ponizej))
        )
    )

    ; 1 argument - paczka do podniesienia 
	(:action podnies-z-podlogi
        :parameters (?klocek)
        :precondition
        (and
            ; nie istnieje paczka w lapie robota 
            (not (exists (?d) (lapa-robota ?d)))
            ; mozna podniesc klocek 
            (mozna-podniesc ?klocek)
            ; klocek lezy na podlodze 
            (podloga ?klocek)
        )
        :effect 
        (and
            ; paczka trafia do lapy robota 
            (lapa-robota ?klocek)
            ; nie mozna juz jej podniesc 
            (not (mozna-podniesc ?klocek))
            ; nie lezy juz na podlodze
            (not (podloga ?klocek))
        )
    )

    ; 1 paczka - cel, jak nie rozpozna to podloga 
    (:action opusc-na-podloge
        :parameters (?klocek)
        :precondition
        (and
            ; klocek aktualnie jest w lapie robota 
            (lapa-robota ?klocek)
        )
        :effect
        (and
            ; klocek juz nie jest w lapie robota 
            (not (lapa-robota ?klocek))
            ; mozna go podniesc 
            (mozna-podniesc ?klocek)
            ; lezy na podlodze 
            (podloga ?klocek)
        )
    )

    ; 1 paczka - cel 
    (:action opusc-na-paczke
        :parameters (?cel ?klocek)
        :precondition
        (and
            ; klocek jest w lapie robota
            (lapa-robota ?klocek)
            ; mozna podniesc cel (jest na gorze stosu lub podlodze)
            (mozna-podniesc ?cel)
        )
        :effect
        (and
            ; klocek nie jest juz w lapie robota 
            (not (lapa-robota ?klocek))
            (not (mozna-podniesc ?cel))
            (mozna-podniesc ?klocek)
            (stoi-na ?klocek ?cel)
        )
    )

)
```

```
(define (problem p1)
	(:domain world-of-blocks)
	(:objects a b c d e)
	(:init
		(mozna-podniesc c)
		(stoi-na c b)
		(stoi-na b a)
		(podloga a)

        (mozna-podniesc e)
        (stoi-na e d)
        (podloga d)
	)
	(:goal
		(and
			(stoi-na d b)
            (exists (?xddd) (stoi-na b ?xddd))
		)
	)
)

; -----

(define (problem p1)
	(:domain world-of-blocks)
	(:objects a b c d e)
	(:init
		(mozna-podniesc c)
		(stoi-na c b)
		(stoi-na b a)
		(podloga a)

        (mozna-podniesc e)
        (stoi-na e d)
        (podloga d)
	)
	(:goal
		(and
			(podloga a)
            (podloga b)
            (podloga c)
            (podloga d)
            (podloga e)
		)
	)
)

; -----


(define (problem p1)
	(:domain world-of-blocks)
	(:objects a b c d e f)
	(:init
		(podloga a)
        (mozna-podniesc a)
        (podloga b)
        (mozna-podniesc b)
        (podloga c)
        (mozna-podniesc c)
        (podloga d)
        (mozna-podniesc d)
        (podloga e)
        (mozna-podniesc e)
        (podloga f)
        (mozna-podniesc f)
	)
	(:goal
		(and
			(exists (?x1) (and
                    (podloga ?x1)
                    (not (exists (?x2) (and
                        (podloga ?x2)
                        (not (= ?x1 ?x2))
                    )))
                    (not (exists (?xd) (lapa-robota ?xd)))
            ))
		)
	)
)

---

(define (problem p1)
	(:domain world-of-blocks)
	(:objects a b c d e f)
	(:init
		(podloga a)
        (mozna-podniesc a)
        (podloga b)
        (mozna-podniesc b)
        (podloga c)
        (mozna-podniesc c)
        (podloga d)
        (mozna-podniesc d)
        (podloga e)
        (mozna-podniesc e)
        (podloga f)
        (mozna-podniesc f)
	)
	(:goal
		(and
			(exists (?x1 ?x2 ?x3 ?x4 ?x5 ?x6) (and
                    (podloga ?x1)
                    (stoi-na ?x2 ?x1)
                    (stoi-na ?x3 ?x2)
                    (stoi-na ?x4 ?x3)
                    (stoi-na ?x5 ?x4)
                    (stoi-na ?x6 ?x5)
            ))
		)
	)
)
```