Here are my thoughts on the "Rational Neural Networks" paper.

After overviewing the main ideas in the "Rational Neural Networks" paper, I think their approach might be better than my original thoughts, simply using a full polynomial for both the numerator and the denominator. Based on empirical results, here are some potential improvements to test out experimentally: 
1. Seeing the empirical success of the ReLU^2 activation function, something interesting to try out would be type (4,2) rational functions. Something about quadratic curvature works well
2. Exploding gradients is problematic. Avoiding poles would help significantly. For the case of (n, 2) functions, it will be very simple to do this since you can calculate the discriminant of the quadratic polynomial in the denominator to ensure it has no real roots. For larger degrees, you can use Sturm's theorem, but it's more costly. Larger degrees could lead to instability, I think their approach of sticking to smaller degrees is good.

Here are some improvements I suggest to the methodology:

In the paper, they use type (3,2) rational functions (degree 3 polynomial in numerator, 2 in denominator) and have 7 parameters for this. However, we can obviously reduce to 6 because we can reparameterize to normalize either the numerator or the denominator. Mathematically, 

$$
\frac{a_3x^3+a_2x^2+a_1x+a_0}{b_2x^2+b_1x+b_0}=\frac{\frac{a_3}{b_2}x^3+\frac{a_2}{b_2}x^2+\frac{a_1}{b_2}x+\frac{a_0}{b_2}}{x^2+\frac{b_1}{b_2}x+\frac{b_0}{b_2}}
$$

Therefore, we only need 6 parameters. This is a very simple optimization to make. It implies that a type $(n, m)$ rational function (degree $n$ polynomial in the numerator, $m$ in the denominator) can be uniquely identified by $n+m+1$ parameters rather than $(n+1)+(m+1)$ that you get from the naive form. 

In practice, models learn better when things are simpler. They are less over-parameterized and by extension less prone to overfitting.

Then, to address the poles, we can leverage the nice properties of quadratic polynomials by rewriting its vertex form, $(x-h)^2+k$. If we force $k>0$, then the quadratic function has no roots! Therefore, we will not have to deal with the headache of poles. So we could have

$$
f(x):=\frac{a_3x^3+a_2x^2+a_1x+a_0}{(x-h)^2+k}
$$

This is maybe a nice indication that quadratic functions might be the nicest to use in the denominator of these rational activation functions. With $k>0$, the entire denominator is also positive ($>0$), which means that the function will be guaranteed continuous on the entire real number line. 

The numerator needs more investigation to see what works best.