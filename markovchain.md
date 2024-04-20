# Proof of the Big Theorem of Markov Chains

In this note we will prove the "Big Theorem" of Discrete Time Markov Chains, i.e. the Existence and Uniqueness of Stationary and Limiting Distributions for a Markov Chain.

Here is the Theorem Statement:

**Thm**: Let $(X_n)_{n\geq 0}$ be an irreducible Markov chain with state space $S$. Exactly one of the following is true:
1. All states are transient or null-recurrent. Then no stationary distribution exists, and $p_{ij}^{(n)}\to 0$ as $n\to\infty$. 
2. All states are positive recurrent. Then a stationary distribution $\pi$ exists and is unique, and satisfies
 $$\pi_j = \lim_{n\to\infty}\frac{1}{n}\sum_{k=1}^np_{ij}^{(k)} = \frac{1}{\mathbb{E}[T_j|X_0=j]}\quad \forall i, j\in S$$
 where $\mathbb{E}[T_j|X_0=j]$ is the expected return time to state $j$. Moreover, if the chain is aperiodic, then $p_{ij}^{(n)}\to\pi_j$ as $n\to\infty$ for all $i, j\in S$. 

 *The proof of this theorem is rather long and involved, so we will approach it in four steps: Setup, which proves some lemmas which will be useful later on in the proof, Existence, which proves the existence of the stationary distribution (2, first statement), Uniqueness, which proves that if you have a stationary distribution, it must be of the form in (2) and that the MC will be positive recurrent, (1, first statement). Finally, we will prove the limiting behavior in (1) and (2)*.

 ___

 ## Setup ##

 In this section, we will prove some necessary lemmas needed to prove all three results. 


 **Lemma 1**: For an irreducible and aperiodic Markov chain, $i, j\in S$, there exists some $N\geq 1$ such that $p_{ij}^{(n)}>0$ for all $n\geq N$. 

 **Proof**: To prove this, we prove this first for $i$ returning to $i$. 
 
 Consider the set $S=\{n\in\mathbb{N}\,:\,p_{ii}^{(n)}>0\}$. Note that $S$ is nonempty since our Markov chain is irreducible. We know that $\gcd(S)=1$. We construct a finite subset $T$ of $S$ that also has a gcd of 1. To do so, let $T_0=\{\min(S)\}$. (This exists since the elements of $S$ are positive integers.) If $\gcd(T_0) = 1$, we are done. Otherwise, there exists some $n_1\in S$ such that $\gcd(T_0\cup\{n_1\})<\gcd(T_0)$, otherwise we would have $\gcd(S)\geq \gcd(T)>1$. Let $T_1=T_0\cup\{n_1\}$. Then if $\gcd(T_1)=1$, we are done. Otherwise, $T_2=T_1\cup\{n_2\}$, where $n_2$ is chosen such that $\gcd(T_2)<\gcd(T_1)$. We continue this process, constructing $T_k=T_{k-1}\cup\{n_k\}$ if necessary. Since we have a decreasing sequence of positive integers, this process will eventually terminate at some $T=T_k$.

 Now, suppose $T=\{n_1, n_2, \ldots, n_k\}$. Since $\gcd(T)=1$, by *Bezout's Lemma*, there exists integers $a_1, a_2, \ldots, a_k$ such that
 $$ a_1n_1+a_2n_2+\ldots+a_kn_k=1.$$
Now, suppose WLOG that $a_1, a_2, \ldots, a_j\geq 0$ and $a_{j+1}, \ldots, a_k<0$ for some $1\leq j\leq k$. Then we can write 
$$a_1n_1+\ldots+a_jn_j=1+|a_{j+1}|n_{j+1}+\ldots+|a_k|n_k.$$
Then let $A = a_1n_1+\ldots+a_jn_j$ and $B=|a_{j+1}|n_{j+1}+\ldots+|a_k|n_k$, so $A, B\geq 0$. If $B=0$, then $A=1$, then we can set $N=1$, since for any $n\geq 1$,
$$p_{ii}^{(n)}\geq \prod_{i=1}^j(p_{ii}^{(n_i)})^{na_i}>0.$$
Otherwise, for let $N = B(B-1)$. Note that $p_{ii}^{(A)}>0$ and $p_{ii}^{(B)}>0$ (by a similar computation as above), and that $A=B+1$. Then for any $n\geq N$, use the division theorem to write $n = qB + r$. Then $q \geq B-1$. Since $r\leq B-1$, this means that $q-r\geq 0$, and we can write 
$$n = (q-r)B+r(B+1)=(q-r)B+rA.$$
However, $p_{ii}^{(n)}\geq (p_{ii}^{(B)})^{q-r}\cdot (p_{ii}^{(A)})^r>0$, as either $q-r>0$ or $r>0$. 

This means we have found an integer $N_i$ such that $p_{ii}^{(n)}>0$ for all $n\geq N_i$. But since our chain is irreducible, $p_{ij}^{(m)}>0$ for some $m\geq 1$. Then setting $N=N_i+m$, we have for $n\geq N$, we have $n-m\geq N_i$, and
$$p_{ij}^{(n)}\geq p_{ii}^{(n-m)}\cdot p_{ij}^{(m)}>0.$$
$\square$

**Lemma 2**: Let $(X_n)$ be an irreducible and recurrent Markov chain. Then for any initial distribution and any state $j$, the hitting time $H_j:=\min\{n\geq 0:X_n = j\}$ is finite with probability 1. 

**Proof**: Suppose $X_0=i$. We will prove that $\mathbb{P}(H_j<\infty|X_0=i)=1$. Since our chain is recurrent, if we start at state $j$, we will revisit state $j$ infinitely many times with probability 1. Also, since our chain is irreducible, $p_{ji}^{(m)}>0$ for some $m
\geq 1$. Then we have
$$\mathbb{P}(X_n = j\text{ for some $n>m$}|X_0 = j)=1.$$
However, this implies that
$$\sum_{k\in S}\mathbb{P}(X_n = j\text{ for some $n>m$}|X_0 = j, X_m = k)\mathbb{P}(X_m=k|X_0=j)=1.$$
$$=\sum_{k\in S}p_{jk}^{(m)}\mathbb{P}(X_n = j\text{ for some $n>m$}|X_0 = j, X_m = k).$$

Here, we have used the law of total probability to rewrite the left hand side. However, we note that $$\mathbb{P}(X_n = j\text{ for some $n>m$}|X_0 = j, X_m = k)=\mathbb{P}(H_j<\infty|X_0=k)$$
since we can use the Markov property to "restart" the Markov chain at $k$ when we reach $k$ after $m$ timesteps. This implies that
$$\sum_{k\in S}p_{jk}^{(m)}\mathbb{P}(H_j<\infty|X_0=k)=1.$$
However, we also have $\sum_{k\in S}p_{jk}^{(m)}=1$, so if $p_{jk}^{(m)}>0$, we must have $\mathbb{P}(H_j<\infty|X_0=k)=1$,(otherwise it would lead to the above inequality being contradicted). Namely, we assumed $p_{ji}^{(m)}>0$, so $\mathbb{P}(H_j<\infty|X_0=i)=1$. $\square$

**Lemma 3**: Define $t_{i}^{(k)}$ to be the expected number of times you visit state $i$ if you start at state $k$, before returning to $k$. Suppose $\boldsymbol{\nu}$ is a vector such that $\boldsymbol{\nu}P=\boldsymbol{\nu}$ and $\nu_k=1$. Then $\nu_j\geq t_j^{(k)}$ for all $j\in S$.

**Proof**: Suppose $\boldsymbol{\nu}$ is a vector such that, i.e. $\boldsymbol{\nu}P=\boldsymbol{\nu}$ and $\nu_k=1$. Then for any state $j\in S$, we can write
$$\nu_j = \sum_{i_0\in S}\nu_{i_0}p_{i_0 j}=p_{kj}+\sum_{i_0\neq k}\nu_{i_0}p_{i_0j}.$$
Now we consider the sum. Now each $\nu_{i_0}$ can be written as a sum, so we can write
$$p_{kj}+\left(\sum_{i_0\neq k}p_{ki_0}p_{i_0j}+\sum_{i_0, i_1\neq k}\nu_{i_1}p_{i_1i_0}p_{i_0j}\right).$$
Now, we can unwrap each $\nu_{i_1}$ similarly and split it into two sums. Then for any $n$, we will have the sum
$$\nu_j=\left(p_{kj}+\sum_{i_0\neq k}p_{ki_0}p_{i_0j}+\ldots+\sum_{i_0, i_1, \ldots, i_{n-1}\neq k}p_{ki_{n-1}}p_{i_{n-1}i_{n-2}}\cdots p_{i_0j}\right)$$
$$+\sum_{i_0, i_1, \ldots, i_n\neq k}\nu_{i_n}p_{i_ni_{n-1}}p_{i_{n-1}i_{n-2}}\cdots p_{i_0j}.$$

$$\geq \left(p_{kj}+\sum_{i_0\neq k}p_{ki_0}p_{i_0j}+\ldots+\sum_{i_0, i_1, \ldots, i_{n-1}\neq k}p_{ki_{n-1}}p_{i_{n-1}i_{n-2}}\cdots p_{i_0j}\right).$$
We now examine each term of this sum. We have that $p_{kj}=\mathbb{P}(X_1=j|X_0=k)=\mathbb{P}(X_1=j, T_k\geq 1|X_0=k)$, where $T_k$ is the return time to $k$, since the return time to $k$ will now be at least 1. The second term is precisely $\mathbb{P}(X_2=j, T_k\geq 2|X_0=k)$, since $X_1=i_0\neq k$. Then
$$v_j\geq\sum_{i=1}^n \mathbb{P}(X_i=j, T_k\geq i|X_0=k)$$
But as $n\to\infty$, we notice that the right hand side must approach $t_j^{(k)}$, since we can view each probability as the expectation of an indicator variable $\mathbb{I}_{\{X_i=j, T_k\geq i\}}$, so we are counting how many states are equal to $j$ before we return to $k$, at which point $T_k\geq i$ will have probaility $0$. So $\nu_j\geq t_j^{(k)}$. $\square$

___

## Existence ##

We prove that a stationary distribution $\boldsymbol{\pi}$ must exist for a positive recurrent chain. Suppose $X_0=k$. We let $\boldsymbol{\nu}$ be vector such that $\nu_j=t_j^{(k)}$ as defined as in Lemma 3, the expected number of times we visit state $j$ before returning to $k$. We claim that $\boldsymbol{\nu}P = \boldsymbol{\nu}$. Then, noting that
$$t_j^{(k)}=\sum_{n=1}^{\infty}\mathbb{P}(X_n=j, T_k\geq n|X_0=k)$$
which is justified in the proof of Lemma 3,
$$\sum_{i\in S}\nu_ip_{ij}=\sum_{i\in S}\sum_{n=1}^{\infty}\mathbb{P}(X_n=i, T_k\geq n|X_0=k)p_{ij}$$
$$=\sum_{n=1}^{\infty}\sum_{i\in S}\mathbb{P}(X_n=i, T_k\geq n|X_0=k)p_{ij}$$
where we are allowed to exchange the sum since $T_k$ is finite with probability 1. 
Now note that the inner sum is 
$$\sum_{n=1}^{\infty}\sum_{i\in S}\mathbb{P}(X_{n}=i, X_{n+1}=j, T_k\geq n|X_0=k)$$
$$=\sum_{n=1}^{\infty}\mathbb{P}(X_{n+1}=j, T_k\geq n|X_0=k).$$
However, since $X_0=X_{T_k}=k$, we can reindex $n$ so that we start counting the visits on the interval $\{0, \ldots, T_k-1\}$ rather than what we are doing, $\{1, \ldots, T_k\}$. Then we can write
$$\sum_{n=0}^{\infty}\mathbb{P}(X_{n+1}=j, T_k-1\geq n|X_0=k)$$
(Note that we do not change $X_{n+1}=j$ since the probabilities will remain the same regardless of our choise of indexing, due to our Markov property.) But now we reindex *again* back to $n=1$, writing
$$\sum_{n=1}^{\infty}\mathbb{P}(X_n=j, T_k\geq n|X_0=k)=\nu_j.$$
Now, we must normalize so that $\boldsymbol{\nu}$ becomes a valid probability distribution. We can do this if $\sum_{i\in S}\nu_i$ is finite. But $\nu_i$ is the expected visits to $i$ before returning to state $k$, so the sum is actually the expected return time to state $k$, which is finite due to positive recurrence. As a result, $\boldsymbol{\pi}=(1/\mathbb{E}[T_k])\boldsymbol{\nu}$ is a valid stationary distribution.

We now prove that if we do have a stationary distribution $\boldsymbol{\pi}$, then our chain must be positive recurrent. Since $\sum_{i\in S}\pi_i=1$, we must have $\pi_k>0$ for some $k\in S$. Now define $\boldsymbol{\nu}=(1/\pi_k)\boldsymbol{\pi}$, which is itself a stationary vector that satisfies $\nu_k=1$. Then
$$\mathbb{E}[T_k|X_0=k]=\sum_{j\in S}t_j^{(k)}\leq\sum_{j\in S}\frac{\pi_j}{\pi_k}=\frac{1}{\pi_k}<\infty$$
as $\pi_k>0$. Here we have directly applied Lemma 3. As a result, the expected return time to $k$ is finite, so $k$ is positive recurrent. Since our chain is irreducible, this implies all states are positive recurrent. $\square$

___

## Uniqueness ##

We prove that the stationary distribution is unique by showing that for a positive recurrent chain, if $\boldsymbol{\pi}$ is a stationary distribution, then $\pi_k=1/\mathbb{E}[T_k|X_0=k]$. 

Let $\mu_k=\mathbb{E}[T_i|X_0=i]$. Then 
$$\mu_k = 1+\sum_{j\in S}p_{kj}H_{jk},$$
where $H_{jk}$ is the expected hitting time to reach $k$ starting from state $j$. Recall that the hitting time starting at $i$ to reach $k$ is given by
$$H_{ik}=1+\sum_{j\in S}p_{ij}H_{jk}$$
for $i\neq k$. Then we can multiply by $\pi_i$ for $i\neq k$ and see that
$$\pi_iH_{ik}=\pi_i+\sum_{j\in S}\pi_ip_{ij}H_{jk}$$
Now we sum over $i$ and get
$$\sum_{i\in S}\pi_iH_{ik}=\sum_{i\neq k}\pi_i+\sum_{j\in S}\sum_{i\neq k}\pi_ip_{ij}H_{jk}.$$
We sum over all $i$ on the left since $H_{kk}=0$. We can also multiply the $\mu_k$ equation by $\pi_k$ to get
$$\pi_k\mu_k=\pi_k+\sum_{j\in S}\pi_kp_{kj}H_{jk}.$$
Now, adding these two equations by 
$$\pi_k\mu_k+\sum_{i\in S}\pi_iH_{ik}=\sum_{i\in S}\pi_i+\sum_{j\in S}\sum_{i\in S}\pi_ip_{ij}H_{jk}.$$
We use the fact that $\sum_{i\in S}\pi_i=1$ and $\sum_{i\in S}\pi_ip_{ij}=\pi_j$ to simplify this expression as
$$\pi_k\mu_k+\sum_{i\in S}\pi_iH_{ik}=1+\sum_{j\in S}\pi_jH_{jk}.$$
Now note that the two summations are the same, and are finite, since $H_{jk}<\infty$ since we have a recurrent chain and Lemma 2. Thus we can cancel out these two terms, and we can see that $\pi_k\mu_k=1$. Then $\pi_k=1/\mu_k$. $\square$

___

##  Limits ##

Let $(X_n)$ be an irreducible, aperiodic, positive recurrent Markov chain. We will prove that $p_{ij}^{(n)}\to \pi_j$ as $n\to\infty$. To do that, we will use a technique called coupling. We know that this Markov Chain has a stationary distribution $\boldsymbol{\pi}$. We will let $(X_n)$ be our Markov chain with any initial distribution, and let $(Y_n)$ be a Markov chain with the same transition probabilities, except that the initial distribution is given by $\boldsymbol{\pi}$. Pick some state $s\in S$. Then let $T$ be the first time $X_n = s$ and $Y_n=s$. Then for $n\geq T$, we will let $X_n = Y_n$. We will now prove that $T<\infty$ with probability 1. Consider now the "coupled" chain $(\mathbf{Z}_n)=(X_n, Y_n)$. Then $(\mathbf{Z}_n)$ is a Markov chain on state space $S\times S$. The transition probabilities are given by 
$$p_{(i_1, i_2)(j_1, j_2)}=p_{i_1j_1}p_{i_2j_2},$$
the probability that $\mathbf{Z}_{n+1}=(i_2, j_2)$ given that $\mathbf{Z}_n=(i_1, j_1)$. Now, for any state $(i_1, j_1)$ and $(i_2, j_2)$, by Lemma 1, there exists some $N_1\geq 1$ such that $p_{i_1j_1}^{(n)}>0$ for $n
\geq N_1$. Similarly, there is some $N_2\geq 1$ such that $p_{i_2j_2}^{(n)}>0$ for $n\geq N_2$. Then for $N=\max(N_1, N_2)$, then 
$$p_{(i_1, i_2)(j_1, j_2)}^{(N)}=p_{i_1j_1}^{(N)}p_{i_2j_2}^{(N)}>0.$$
so $(\mathbf{Z}_n)$ is irreducible. Furthermore, if we let 
$$\pi_{(i_1, i_2)}=\pi_{i_1}\pi_{i_2}$$
then
$$\sum_{(i_1, i_2)\in S\times S}\pi_{(i_1, i_2)}p_{(i_1, i_2)(j_1, j_2)}=\left(\sum_{i_1\in S}\pi_{i_1}p_{i_1j_1}\right)\left(\sum_{i_2\in S}\pi_{i_2}p_{i_2j_2}\right)=\pi_{i_1}\pi_{i_2},$$
so a stationary distribution exists on $(\mathbf{Z}_n)$. Then since we have a stationary distribution on an irreducible chain, by the second part of Existence, $(\mathbf{Z}_n)$ is positive recurrent. Then $T$ is finite with probability 1, by Lemma 2.

Now, suppose $(X_0, Y_0)=(i, j). Then for some $n$, 
$$p_{ik}^{(n)}=\mathbb{P}(X_n=k, T\leq n)+\mathbb{P}(X_n=k, T> n)$$
$$=\mathbb{P}(Y_n=k, T\leq n)+\mathbb{P}(X_n=k, T> n)$$
since if $n\geq T$, then $X_n=Y_n$ (this is the coupling idea). Now we can write
$$p_{jk}^{(n)}+\mathbb{P}(X_n=k, T>n)$$
$$\leq p_{jk}^{(n)} +\mathbb{P}(T>n).$$
This means $$p_{ik}^{(n)}-p_{jk}^{(n)}\leq \mathbb{P}(T>n).$$
But since $T$ is finite with probability 1, $\mathbb{P}(T>n)\to 0$ as $n\to\infty$. Then $p_{ik}^{(n)}-p_{jk}^{(n)}\to 0$. Then
$$\pi_k-p_{jk}^{(n)}=\sum_{i\in S}\pi_i(p_{ik}^{(n)}-p_{jk}^{(n)})$$
where we have used the fact that $\sum_{i\in S}\pi_i=1$ and $\sum_{i\in S}\pi_ip_{ik}^{(n)}=\pi_k$. (since $\boldsymbol{\pi}P^n =\boldsymbol{\pi}$) But using our limit from before, this implies $p_{jk}^{(n)}\to \pi_k$ as $n\to\infty$. 

If our chain is null recurrent or transient, then we prove that we cannot have $p_{ij}^{(n)}\to \ell_j$ for $\ell_j>0$ for any $j\in S$. If that were the case, then we can construct a distribution $\mathbf{\ell}^*$ by normalizing the vector $\mathbf{\ell}$. We show that this implies a stationary distribution. Then

$$\sum_{i\in S}\ell_ip_{ij}=\sum_{i\in S}\lim_{n\to\infty}p_{ki}^{(n)}p_{ij}=\lim_{n\to\infty}\sum_{i\in S}p_{ki}^{(n)}p_{ij}=\lim_{n\to\infty}p_{kj}^{(n+1)}=\ell_j$$

so $\mathbf{\ell}$ is a stationary vector, which implies a stationary distribution, which is not possible for a null recurrent or transient chain. Then $p_{ij}^{(n)}\to 0$ as $n\to\infty$ for all $i, j\in S$.

___
## Limit 2 ##
We prove the final limit in the second part of the Big Theorem. 

Let $V_j(n)$ be the number of visits to a state $j\in S$ up to time $n$. Since our chain is positive recurrent, we will eventually reach $j$ in some finite time with probability 1, so assume without loss of generality that $X_0=j$. Let $T_j^{(r)}$ be the amount of time between the $r$ and $r+1$-th visits to $j$. The note that due to the Markov Property, the $T_j^{(r)}$ are i.i.d for each $r$, and $\mathbb{E}[T_j^{(r)}]=\mu_j$, where $T_j$ is the expected return time to $j$. Then the time of the last visit before time $n$ is 
$$T_j^{(1)}+T_j^{(2)}+\ldots+T_j^{(V_j(n)-1)}<n$$
Also, the time of the first visit after time $n$ is 
$$T_j^{(1)}+T_j^{(2)}+\ldots+T_j^{(V_j(n))}\geq n$$
Then 
$$\frac{T_j^{(1)}+T_j^{(2)}+\ldots+T_j^{(V_j(n)-1)}}{V_j(n)}\leq\frac{n}{V_j(n)}\leq\frac{T_j^{(1)}+T_j^{(2)}+\ldots+T_j^{(V_j(n))}}{V_j(n)}$$
Now, $V_j(n)\to\infty$ as $n\to\infty$ since we have a positive recurrent chain, as we will keep returning to $j$. Then by the Strong Law of Large Numbers, the left and right limits are $\mu_j$, so $V_j(n)/n$ converges to $1/\mu_j=\pi_j$ almost surely. Now, for any starting state $i\in S$,
$$\mathbb{E}[V_j(n)]=\sum_{k=1}^np_{ij}^{(k)},$$
but this means that
$$\mathbb{E}\left[\frac{V_j(n)}{n}\right]=\frac{1}{n}\sum_{k=1}^np_{ij}^{(k)}\to\pi_j.$$
$\square$
___
This concludes the proof of the Big Theorem.
