Cтворити з’єдання з базою даних
```r
conn <- dbConnect(RSQLite::SQLite(), "database.sqlite")
```
Необхідно створити фрейми даних:
1. Назва статті (Title), тип виступу (EventType). Необхідно вибрати тільки статті с типом виступу Spotlight. Сортування по назві статті.
```r
res<-dbSendQuery(conn,"SELECT Title, EventType FROM Papers WHERE EventType='Spotlight' ORDER BY Title")
df<-dbFetch(res,n=10)
df
```
```r
 Title EventType
1  A Tractable Approximation to Optimal Point Process Filtering: Application to Neural Encoding Spotlight
2                                    Accelerated Mirror Descent in Continuous and Discrete Time Spotlight
3                        Action-Conditional Video Prediction using Deep Networks in Atari Games Spotlight
4                                                                      Adaptive Online Learning Spotlight
5                          Asynchronous Parallel Stochastic Gradient for Nonconvex Optimization Spotlight
6                                                 Attention-Based Models for Speech Recognition Spotlight
7                                                       Automatic Variational Inference in Stan Spotlight
8                                   Backpropagation for Energy-Efficient Neuromorphic Computing Spotlight
9                       Bandit Smooth Convex Optimization: Improving the Bias-Variance Tradeoff Spotlight
10                         Biologically Inspired Dynamic Textures for Probing Motion Perception Spotlight
```
2. Ім’я автора (Name), Назва статті (Title). Необхідно вивести всі назви статей для автора «Josh Tenenbaum». Сортування по назві статті.
```r
res<-dbSendQuery(conn,"SELECT a.Name AS Author, p.Title FROM PaperAuthors p_a INNER JOIN Authors a  
ON a.Id=p_a.AuthorId INNER JOIN Papers p ON p.Id=p_a.PaperId WHERE a.Name = 'Josh Tenenbaum' ORDER BY p.Title")
df<-dbFetch(res,n=10)
df
```
```r
          Author                                                                                             Title
1 Josh Tenenbaum                                                       Deep Convolutional Inverse Graphics Network
2 Josh Tenenbaum Galileo: Perceiving Physical Object Properties by Integrating a Physics Engine with Deep Learning
3 Josh Tenenbaum                                                Softstar: Heuristic-Guided Probabilistic Inference
4 Josh Tenenbaum                                                        Unsupervised Learning by Program Synthesis
```
3. Вибрати всі назви статей (Title), в яких є слово «statistical». Сортування по назві статті.
```r
res<-dbSendQuery(conn,"SELECT Title FROM Papers WHERE Title LIKE '%statistical%' ORDER BY Title")
df<-dbFetch(res, n=10)
df
```
```r                                                                          
                                                                                Title
1 Adaptive Primal-Dual Splitting Methods for Statistical Learning and Image Processing
2                                Evaluating the statistical significance of biclusters
3                  Fast Randomized Kernel Ridge Regression with Statistical Guarantees
4     High Dimensional EM Algorithm: Statistical Optimization and Asymptotic Normality
5                Non-convex Statistical Optimization for Sparse Tensor Graphical Model
6            Regularized EM Algorithms: A Unified Framework and Statistical Guarantees
7                            Statistical Model Criticism using Kernel Two Sample Tests
8                         Statistical Topological Data Analysis - A Kernel Perspective
```
4. Ім’я автору (Name), кількість статей по кожному автору (NumPapers). Сортування по кількості статей від більшої кількості до меньшої.
```r
res<-dbSendQuery(conn,"SELECT distinct(a.Name) AS Author, count(p_a.PaperId) AS NumPapers FROM PaperAuthors p_a 
INNER JOIN Authors a ON a.Id=p_a.AuthorId GROUP BY a.Name ORDER BY NumPapers DESC")
df<-dbFetch(res, n=10)
df
```
```r
                 Author NumPapers
1  Pradeep K. Ravikumar         7
2        Lawrence Carin         6
3               Han Liu         6
4     Zoubin Ghahramani         5
5               Le Song         5
6   Inderjit S. Dhillon         5
7          Zhaoran Wang         4
8         Yoshua Bengio         4
9  Simon Lacoste-Julien         4
10          Shie Mannor         4
```
