# Data analysis on treatment effectiveness in Crohn's Disease-related adverse events
Treatment effects' predictions using data from the National Cooperative Crohn's Disease Study (NCCDS)

We conducted **Bayesian multilevel models** to investigate the effectiveness of different treatments to reduce adverse events in people with Crohn's Disease (CD) using the code below.

Model 1 using a *skew_normal()* likelihood

`model_1 <- brm(number_of_adverse_events ~ 1 + sex + age + BMI + treatment + (1 | ID),
              data = CrohnD,
              family = skew_normal(),
              chains = 4,
              iter = 3000,
              seed = 23102024)`

Model 2 using a *Poisson* likelihood

`model_2 <- brm(number_of_adverse_events ~ 1 + sex + age + BMI + treatment + (1 | ID),
              data = CrohnD,
              family = "poisson",
              chains = 4,
              iter = 3000,
              seed = 23102024)`

Next, to ensure that our model fits our observed data well, predictive posterior checkings were performed.

![plot_cronh_pp](https://github.com/user-attachments/assets/ab784887-d43d-4ae6-a324-6790494b09a7)


After that, we can predict the number of adverse events adjusting by the body mass index (BMI) and the age of the patient using the outstanding `marginaleffects` package. Here we can see that more age and BMI were associated to a larger number of adverse events.

`pred <- predictions(model_2,
                     newdata = datagrid(BMI = seq(20, 30, 1),
                                        sex = c("F", "M"),
                                        age = seq(30, 70, 1),
                                        treat = c("d1", "d2", "placebo"))) %>%
        posteriordraws()`

![cronh](https://github.com/user-attachments/assets/9705539d-af68-4b29-b9ef-0ee770da3497)

Furthermore, it would be helpful for clinicians and decision-makers to know whether females with CD are more predisposed to suffer adverse events or not; and obviously, which treatment showed fewer adverse events. Thus, we visualize these estimates. With this kind of plots (see above), we can miss information about the uncertainty of the estimates. Therefore, we present the predictions using density and dot plots.

![plot_cronh_p2](https://github.com/user-attachments/assets/2c0d2f1a-d225-47ff-8e14-f173788004db)
