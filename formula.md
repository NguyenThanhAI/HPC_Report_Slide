$$\bold{y} = \lbrace \bold{y}_1, \dots, \bold{y}_C \rbrace, \bold{y}_i \in \mathbb{R}^K$$

$$\bold{a}=\lbrace \bold{a}_1, \dots, \bold{a}_L \rbrace, \bold{a}_i \in \mathbb{R}^D$$

$$T_{s,t}: \mathbb{R}^s \rightarrow \mathbb{R}^t$$

$$\begin{bmatrix}
    \bold{i}_t \\
    \bold{f}_t \\
    \bold{o}_t \\
    \bold{g}_t
\end{bmatrix}=\begin{bmatrix}
    \sigma \\
    \sigma \\
    \sigma \\
    \mathrm{tanh}
\end{bmatrix}T_{D+m+n,n}\begin{bmatrix}
    \bold{Ey}_{t-1} \\
    \bold{h}_{t-1} \\
    \hat{\bold{z}}_t
\end{bmatrix}$$

$$\bold{c}_t = \bold{f}_t \odot\bold{c}_{t-1} + \bold{i}_t \odot \bold{g}_t$$

$$\bold{h}_t = \bold{o}_t \odot \mathrm{tanh}(\bold{c}_t)$$

$$\hat{\bold{z}}_t \in \mathbb{R}^D$$

$$\bold{E} \in \mathbb{R}^{m\times K}$$

$$e_{ti}=f_{\mathrm{att}}(\bold{a}_i, \bold{h}_{t-1})$$

$$\alpha_{ti}=\dfrac{\exp(e_{ti})}{\displaystyle\sum_{k=1}^L \exp(e_{tk})}$$

$$\hat{\bold{z}}_t = \phi (\lbrace \bold{a}_i \rbrace, \lbrace \alpha_i \rbrace) $$

$$\bold{c}_0 = f_{\mathrm{init, c}}\big(\dfrac{1}{L}\sum_i^L\bold{a}_i\big)$$

$$\bold{h}_0 = f_{\mathrm{init, h}}\big(\dfrac{1}{L}\sum_i^L\bold{a}_i\big)$$


$$p\big(\bold{y}_t\vert \bold{a}, \bold{y}_1^{t-1}\big) \propto \exp\big( \bold{L}_0(\bold{Ey}_{t-1} + \bold{L}_h\bold{h}_t + \bold{L}_z \hat{\bold{z}}_t) \big)$$

$$\bold{L}_0 \in \mathbb{R}^{K\times m}$$

$$\bold{L}_h \in \mathbb{R}^{m\times n}$$

$$\bold{L}_z \in \mathbb{R}^{m\times D}$$

$$p(s_{t, i}=1 \vert s_{j < t}, \bold{a})=\alpha_{t,i}$$

$$\hat{\bold{z}}_t = \sum_i s_{t,i}\bold{a}_i$$

$$L_s = \sum_s p(s\vert \bold{a}) \log p(\bold{y}\vert s, \bold{a})\\\leq \log \sum_s p(s \vert \bold{a}) p (\bold{y} \vert s, \bold{a})\\=\log p (\bold{y} \vert \bold{a})$$

$$\dfrac{\partial L_s}{\partial W}=\sum_s p(s \vert \bold{a}) \Big\lbrack \dfrac{\partial \log p(\bold{y} \vert s, \bold{a})}{\partial W} + \log p(\bold{y} \vert s, \bold{a}) \dfrac{\partial p(s \vert \bold{a})}{\partial W} \Big\rbrack)$$

$$\tilde{s}_t \sim \mathrm{Multinoulli}_L \big(\lbrace \alpha_i \big\rbrace)$$

$$\dfrac{\partial L_s}{\partial W} \approx \dfrac{1}{N}\sum_{n=1}^N \Big\lbrack \dfrac{\partial \log p(\bold{y} \vert \tilde{s}^n, \bold{a})}{\partial W} + \log p(\bold{y} \vert \tilde{s}^n, \bold{a}) \dfrac{\partial p(\tilde{s}^n \vert \bold{a})}{\partial W} \Big\rbrack)$$

$$b_k = 0.9 \times b_{k-1} + 0.1 \times \log p (y \vert \tilde{s}_k, \bold{a})$$


$$\dfrac{\partial L_s}{\partial W}=\dfrac{1}{N}\sum_{n=1}^N \Big\lbrack \dfrac{\partial \log p(\bold{y} \vert \tilde{s}^n, \bold{a})}{\partial W} + \lambda_r (\log p(\bold{y} \vert \tilde{s}^n, \bold{a}) - b)\dfrac{\partial p(\tilde{s}^n \vert \bold{a})}{\partial W} + \lambda_e \dfrac{\partial H \lbrack \tilde{s}^n \rbrack}{\partial W} \Big \rbrack $$


$$\mathbb{E}_{p(s_t \vert a)}\lbrack \hat{\bold{z}}_t \rbrack=\sum_{i=1}^L \alpha_{t,i}\bold{a}_i$$

$$\mathbb{E}_{p(s_t \vert a)}\lbrack \bold{h}_t \rbrack$$


$$\bold{n}_t = \bold{L}_0(\bold{Ey}_{t-1} + \bold{L}_h\bold{h}_t + \bold{L}_z \hat{\bold{z}}_t)$$

$$\mathrm{NWGM}\lbrack p(y_t=k\vert a) \rbrack=\dfrac{\displaystyle \prod_i \exp\big( n_{t, k, i} \big)^{p(s_{t,i}=1\vert a)}}{\displaystyle \sum_j \prod_i \exp\big( n_{t, j, i} \big)^{p(s_{t,i}=1\vert a)}}=\dfrac{\exp\big( \mathbb{E}_{p(s_t \vert a)} \lbrack n_{t,k} \rbrack  \big)}{\displaystyle \sum_j \exp \big(\mathbb{E}_{p(s_t \vert a)} \lbrack n_{t,j} \rbrack \big)}\approx \mathbb{E} \lbrack p(y_t=k \vert a)\rbrack$$

$$\mathbb {E}\lbrack \bold{n}_t \rbrack = \bold{L}_0(\bold{Ey}_{t-1} + \bold{L}_h \mathbb{E} \lbrack \bold{h}_t \rbrack + \bold{L}_z \mathbb{E} \lbrack \hat{\bold{z}}_t) \rbrack$$

$$\sum_i \alpha_{ti}=1$$

$$\hat{\bold{z}}_t=\phi\big( \lbrace \bold{a}_i \rbrace, \lbrace \alpha_{ti} \rbrace \big)=\beta \sum_{i}^L \alpha_{ti} \bold{a}_i$$

$$L_d=-\log\big( P(\bold{y} \vert \bold{x}) \big) + \lambda \sum_i^L (1-\sum_t^C \alpha_{ti})^2$$