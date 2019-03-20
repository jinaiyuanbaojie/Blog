# 数学原理  
## 前提
### 矩阵加法&乘法     
- 加法  
$$
  \begin{bmatrix}
    a_{11} & a_{12} & a_{13}\\
    a_{21} & a_{22} & a_{23}\\
    a_{31} & a_{32} & a_{33}\\
  \end{bmatrix} +
  \begin{bmatrix}
    b_{11} & b_{12} & b_{13}\\
    b_{21} & b_{22} & b_{23}\\
    b_{31} & b_{32} & b_{33}\\
  \end{bmatrix} = 
  \begin{bmatrix}
    a_{11}+b_{11} & a_{12}+b_{12} & a_{13}+b_{13}\\
    a_{21}+b_{21} & a_{22}+b_{22} & a_{23}+b_{23}\\
    a_{31}+b_{31} & a_{32}+b_{32} & a_{33}+b_{33}\\
  \end{bmatrix}
$$

- 乘法   
$$
  \begin{bmatrix}
    a_{11} & a_{12} & a_{13}\\
    a_{21} & a_{22} & a_{23}\\
    a_{31} & a_{32} & a_{33}\\
  \end{bmatrix} *
  \begin{bmatrix}
    b_{11} & b_{12} & b_{13}\\
    b_{21} & b_{22} & b_{23}\\
    b_{31} & b_{32} & b_{33}\\
  \end{bmatrix} = 
  {\begin{pmatrix}
    AB
  \end{pmatrix}}_{ij}={\sum_{k=1}^3 a_{ik}*b_{kj}}
$$

e.g.   
$$
  \begin{bmatrix}
    1 & 2 & 1\\
    0 & 2 & 2\\
    3 & 2 & 1\\
  \end{bmatrix} *
  \begin{bmatrix}
    2 & 2 & 3\\
    3 & 2 & 1\\
    0 & 1 & 0\\
  \end{bmatrix} = 
  \begin{bmatrix}
    8 & 7 & 5\\
    6 & 6 & 2\\
    12 & 11 & 11\\
  \end{bmatrix}=
  \begin{bmatrix}
    1*2+2*3+1*0 & 1*2+2*2+1*1 & 1*3+2*1+1*0\\
    0*2+2*3+2*0 & 0*2+2*2+2*1 & 0*3+2*1+2*0\\
    3*2+2*3+1*0 & 3*2+2*2+1*1 & 3*3+2*1+1*0\\
  \end{bmatrix}
$$

- 单位矩阵 
$$
E=\begin{bmatrix}
    1 & 0 & 0\\
    0 & 1 & 0\\
    0 & 0 & 1\\
  \end{bmatrix}
$$

$$
A=A*E=E*A
$$

> Tips:  
> 1.矩阵源于解线性方程组；    
2.乘法限制 $A_{mn}*B_{np}$； A的列数等于B的行数；  
3.满足结合率 (AB)C=A(BC)；  
4.不满足交换律 AB不一定等于BA；     

### 三角函数和差公式
$$
sin(\alpha\pm\beta) = sin{\alpha}cos{\beta} \pm cos{\alpha}sin{\beta}
$$
$$
cos(\alpha\pm\beta) = cos{\alpha}cos{\beta} \mp sin{\alpha}sin{\beta}
$$
> 推导旋转矩阵用        

## 运动学公式  
> 描述物体或者点在空间中的位置

### 平移变换及矩阵    
二维平面上的某点$(x_0,y_0)$平移$(x_t,y_t)$后，得到点$(x_1,y_1)$
$$
(x_1,y_1) = (x_0,y_0) + (x_t,y_t) = (x_0+x_t,y_0+y_t)
$$
表示为矩阵的形式
$$
\begin{bmatrix}
    x_1 \\
    y_1 \\
    1 \\
  \end{bmatrix} = 
\begin{bmatrix}
    1 & 0 & x_t\\
    0 & 1 & y_t\\
    0 & 0 & 1\\
  \end{bmatrix} *
\begin{bmatrix}
    x_0 \\
    y_0 \\
    1 \\
  \end{bmatrix}
$$
> Tips:  
> 1.扩展后的矩阵是齐次变换矩阵；   
> 2.用矩阵描述可以统一运动学模型，见下面的变换矩阵；  

### 旋转变换及矩阵
二维平面上有点$(x_0,y_0)$与$x$轴夹角为$\alpha$，逆时针旋转$\beta$后得到点$(x_1,y_1)$。  
令
$$
r =  \sqrt{{x_0}^2 + {y_0}^2}
$$

有
$$
x_0 = rcos{\alpha}
$$

$$
y_0 = rsin{\alpha}
$$

$$
x_1 = rcos(\alpha + \beta) = rcos{\alpha}cos{\beta} - rsin{\alpha}sin{\beta} = x_0cos{\beta} - y_0sin{\beta}
$$

$$
y_1 = rsin(\beta + \alpha) = rcos{\alpha}sin{\beta} + rsin{\alpha}cos{\beta} = x_0sin{\beta} + y_0cos{\beta}
$$

写为矩阵的形式，有
$$
\begin{bmatrix}
    x_1 \\
    y_1 \\
    1 \\
  \end{bmatrix} = 
\begin{bmatrix}
    cos{\beta} & -sin{\beta} & 0\\
    sin{\beta} & cos{\beta} & 0\\
    0 & 0 & 1\\
  \end{bmatrix} *
\begin{bmatrix}
    x_0 \\
    y_0 \\
    1 \\
  \end{bmatrix}
$$

### 缩放变换及矩阵  
指点$(x_0,y_0)沿 $x$ 轴和 $y$ 轴放大或缩小  
$$
x_1 = k_1x_0,  y_1 = k_2y_0
$$
矩阵形式       
$$
\begin{bmatrix}
    x_1 \\
    y_1 \\
    1 \\
  \end{bmatrix} = 
\begin{bmatrix}
    k_1 & 0 & 0\\
    0 & k_2 & 0\\
    0 & 0 & 1\\
  \end{bmatrix} *
\begin{bmatrix}
    x_0 \\
    y_0 \\
    1 \\
  \end{bmatrix}
$$

### 组合变换及矩阵    
> 仿射(Affine)变换：变换矩阵可以表示为上述矩阵乘积的形式.  

任何复杂的变换都可以表示成上述三种矩阵乘积的形式。e.g: 点$P(x_1,y_1)$绕着$(x_0,y_0)$旋转$\alpha$，可以表示成如下形式：    
$$
   T(x_0,y_0)R(\alpha)T(-x_0,-y_0)P
$$

## 结论    
忘记推导，记住结论  
所有的运动都可以分解为上述运动的组合，表现为矩阵相乘的形式。

# Android&iOS动画实践 
见示例代码

# Android&iOS动画源码及API清单      
> 帧率，丢帧，参考系，世界坐标系
## Android(API 27)  
### API清单
- Matrix  
```java
public class Matrix {

    public static final int MSCALE_X = 0;   //!< use with getValues/setValues
    public static final int MSKEW_X  = 1;   //!< use with getValues/setValues
    public static final int MTRANS_X = 2;   //!< use with getValues/setValues
    public static final int MSKEW_Y  = 3;   //!< use with getValues/setValues
    public static final int MSCALE_Y = 4;   //!< use with getValues/setValues
    public static final int MTRANS_Y = 5;   //!< use with getValues/setValues
    public static final int MPERSP_0 = 6;   //!< use with getValues/setValues
    public static final int MPERSP_1 = 7;   //!< use with getValues/setValues
    public static final int MPERSP_2 = 8;   //!< use with getValues/setValues

    /** @hide */
    public final static Matrix IDENTITY_MATRIX = new Matrix()....

    /** Set the matrix to translate by (dx, dy). */
    public void setTranslate(float dx, float dy) {
        nSetTranslate(native_instance, dx, dy);
    }

    /**
     * Set the matrix to rotate by the specified number of degrees, with a pivot point at (px, py).
     * The pivot point is the coordinate that should remain unchanged by the specified
     * transformation.
     */
    public void setRotate(float degrees, float px, float py) {
        nSetRotate(native_instance, degrees, px, py);
    }

    /**
     * Set the matrix to rotate about (0,0) by the specified number of degrees.
     */
    public void setRotate(float degrees) {
        nSetRotate(native_instance, degrees);
    }

    ...

    /**
     * Set the matrix to the concatenation of the two specified matrices and return true.
     * <p>
     * Either of the two matrices may also be the target matrix, that is
     * <code>matrixA.setConcat(matrixA, matrixB);</code> is valid.
     * </p>
     * <p class="note">
     * In {@link android.os.Build.VERSION_CODES#GINGERBREAD_MR1} and below, this function returns
     * true only if the result can be represented. In
     * {@link android.os.Build.VERSION_CODES#HONEYCOMB} and above, it always returns true.
     * </p>
     */
    public boolean setConcat(Matrix a, Matrix b) {
        nSetConcat(native_instance, a.native_instance, b.native_instance);
        return true;
    }

    /**
     * Preconcats the matrix with the specified translation. M' = M * T(dx, dy)
     */
    public boolean preTranslate(float dx, float dy) {
        nPreTranslate(native_instance, dx, dy);
        return true;
    }

    /**
     * Preconcats the matrix with the specified matrix. M' = M * other
     */
    public boolean preConcat(Matrix other) {
        nPreConcat(native_instance, other.native_instance);
        return true;
    }

    /**
     * Postconcats the matrix with the specified translation. M' = T(dx, dy) * M
     */
    public boolean postTranslate(float dx, float dy) {
        nPostTranslate(native_instance, dx, dy);
        return true;
    }

    ...

    /**
     * Postconcats the matrix with the specified matrix. M' = other * M
     */
    public boolean postConcat(Matrix other) {
        nPostConcat(native_instance, other.native_instance);
        return true;
    }

    ...JNI APIs
}
```
   
- Transformation.  
```java
public class Transformation {
    ...

    protected Matrix mMatrix;

    ...

    /**
     * Clones the specified transformation.
     *
     * @param t The transformation to clone.
     */
    public void set(Transformation t) {
        mAlpha = t.getAlpha();
        mMatrix.set(t.getMatrix());
        if (t.mHasClipRect) {
            setClipRect(t.getClipRect());
        } else {
            mHasClipRect = false;
            mClipRect.setEmpty();
        }
        mTransformationType = t.getTransformationType();
    }

    /**
     * Apply this Transformation to an existing Transformation, e.g. apply
     * a scale effect to something that has already been rotated.
     * @param t
     */
    public void compose(Transformation t) {
        mAlpha *= t.getAlpha();
        mMatrix.preConcat(t.getMatrix());
        if (t.mHasClipRect) {
            Rect bounds = t.getClipRect();
            if (mHasClipRect) {
                setClipRect(mClipRect.left + bounds.left, mClipRect.top + bounds.top,
                        mClipRect.right + bounds.right, mClipRect.bottom + bounds.bottom);
            } else {
                setClipRect(bounds);
            }
        }
    }
    
    /**
     * Like {@link #compose(Transformation)} but does this.postConcat(t) of
     * the transformation matrix.
     * @hide
     */
    public void postCompose(Transformation t) {
        mAlpha *= t.getAlpha();
        mMatrix.postConcat(t.getMatrix());
        if (t.mHasClipRect) {
            Rect bounds = t.getClipRect();
            if (mHasClipRect) {
                setClipRect(mClipRect.left + bounds.left, mClipRect.top + bounds.top,
                        mClipRect.right + bounds.right, mClipRect.bottom + bounds.bottom);
            } else {
                setClipRect(bounds);
            }
        }
    }

    /**
     * @return The 3x3 Matrix representing the trnasformation to apply to the
     * coordinates of the object being animated
     */
    public Matrix getMatrix() {
        return mMatrix;
    }

    ...
}
```
- Animation. 
```java
public abstract class Animation implements Cloneable {
    ...
    public boolean getTransformation(long currentTime, Transformation outTransformation) {
        if (mStartTime == -1) {
            mStartTime = currentTime;
        }

        final long startOffset = getStartOffset();
        final long duration = mDuration;
        float normalizedTime;
        if (duration != 0) {
            normalizedTime = ((float) (currentTime - (mStartTime + startOffset))) /
                    (float) duration;
        } else {
            // time is a step-change with a zero duration
            normalizedTime = currentTime < mStartTime ? 0.0f : 1.0f;
        }

        final boolean expired = normalizedTime >= 1.0f || isCanceled();
        mMore = !expired;

        if (!mFillEnabled) normalizedTime = Math.max(Math.min(normalizedTime, 1.0f), 0.0f);

        if ((normalizedTime >= 0.0f || mFillBefore) && (normalizedTime <= 1.0f || mFillAfter)) {
            if (!mStarted) {
                fireAnimationStart();
                mStarted = true;
                if (NoImagePreloadHolder.USE_CLOSEGUARD) {
                    guard.open("cancel or detach or getTransformation");
                }
            }

            if (mFillEnabled) normalizedTime = Math.max(Math.min(normalizedTime, 1.0f), 0.0f);

            if (mCycleFlip) {
                normalizedTime = 1.0f - normalizedTime;
            }

            final float interpolatedTime = mInterpolator.getInterpolation(normalizedTime);
            applyTransformation(interpolatedTime, outTransformation);
        }
      ...
    }

    ...

    public boolean getTransformation(long currentTime, Transformation outTransformation,
            float scale) {
        mScaleFactor = scale;
        return getTransformation(currentTime, outTransformation);
    }

    protected void applyTransformation(float interpolatedTime, Transformation t) {
    }

    protected float resolveSize(int type, float value, int size, int parentSize) {
        switch (type) {
            case ABSOLUTE:
                return value;
            case RELATIVE_TO_SELF:
                return size * value;
            case RELATIVE_TO_PARENT:
                return parentSize * value;
            default:
                return value;
        }
    }
  
    ...
}
```

- TranslateAnimation. 
```java
public class TranslateAnimation extends Animation {
    private int mFromXType = ABSOLUTE;
    private int mToXType = ABSOLUTE;

    private int mFromYType = ABSOLUTE;
    private int mToYType = ABSOLUTE;

    /** @hide */
    protected float mFromXValue = 0.0f;
    /** @hide */
    protected float mToXValue = 0.0f;

    /** @hide */
    protected float mFromYValue = 0.0f;
    /** @hide */
    protected float mToYValue = 0.0f;

    /** @hide */
    protected float mFromXDelta;
    /** @hide */
    protected float mToXDelta;
    /** @hide */
    protected float mFromYDelta;
    /** @hide */
    protected float mToYDelta;

    ...

    @Override
    protected void applyTransformation(float interpolatedTime, Transformation t) {
        float dx = mFromXDelta;
        float dy = mFromYDelta;
        if (mFromXDelta != mToXDelta) {
            dx = mFromXDelta + ((mToXDelta - mFromXDelta) * interpolatedTime);
        }
        if (mFromYDelta != mToYDelta) {
            dy = mFromYDelta + ((mToYDelta - mFromYDelta) * interpolatedTime);
        }
        t.getMatrix().setTranslate(dx, dy);
    }

    ...
}
```

- RotateAnimation. 
```java
public class RotateAnimation extends Animation {
    private float mFromDegrees;
    private float mToDegrees;

    private int mPivotXType = ABSOLUTE;
    private int mPivotYType = ABSOLUTE;
    private float mPivotXValue = 0.0f;
    private float mPivotYValue = 0.0f;

    private float mPivotX;
    private float mPivotY;

    @Override
    protected void applyTransformation(float interpolatedTime, Transformation t) {
        float degrees = mFromDegrees + ((mToDegrees - mFromDegrees) * interpolatedTime);
        float scale = getScaleFactor();
        
        if (mPivotX == 0.0f && mPivotY == 0.0f) {
            t.getMatrix().setRotate(degrees);
        } else {
            t.getMatrix().setRotate(degrees, mPivotX * scale, mPivotY * scale);
        }
    }
}
```

- AnimationSet. 
```java
public class AnimationSet extends Animation {

    ...

    private ArrayList<Animation> mAnimations = new ArrayList<Animation>();

    private Transformation mTempTransformation = new Transformation();

    @Override
    public boolean getTransformation(long currentTime, Transformation t) {
        final int count = mAnimations.size();
        final ArrayList<Animation> animations = mAnimations;
        final Transformation temp = mTempTransformation;

        boolean more = false;
        boolean started = false;
        boolean ended = true;

        t.clear();

        for (int i = count - 1; i >= 0; --i) {
            final Animation a = animations.get(i);

            temp.clear();
            more = a.getTransformation(currentTime, temp, getScaleFactor()) || more;
            t.compose(temp);

            started = started || a.hasStarted();
            ended = a.hasEnded() && ended;
        }
        
        ...
    }
    
    ...
}
```

### 插值器 & 差值函数  
- 线性插值器   
$$y=x$$
```java
public class LinearInterpolator extends BaseInterpolator implements NativeInterpolatorFactory {
    
    public float getInterpolation(float input) {
        return input;
    }    
}
```

![](https://ws3.sinaimg.cn/large/006tNbRwly1fvf4v99v2fj31gg0pcad2.jpg).  
- 加速插值器   
$$y=x^2$$
```java
public class AccelerateInterpolator extends BaseInterpolator implements NativeInterpolatorFactory {
    private final float mFactor;
    private final double mDoubleFactor;

    public AccelerateInterpolator() {
        mFactor = 1.0f;
        mDoubleFactor = 2.0;
    }

    public AccelerateInterpolator(float factor) {
        mFactor = factor;
        mDoubleFactor = 2 * mFactor;
    }

    public float getInterpolation(float input) {
        if (mFactor == 1.0f) {
            return input * input;
        } else {
            return (float)Math.pow(input, mDoubleFactor);
        }
    }
}
```

![](https://ws1.sinaimg.cn/large/006tNbRwly1fvf4v8yjpuj31gq0q077q.jpg)
- 加速减速插值器   
$$y=cos((x+1)*\pi)/2+0.5$$ 
```java
public class AccelerateDecelerateInterpolator extends BaseInterpolator
        implements NativeInterpolatorFactory {
    
    public float getInterpolation(float input) {
        return (float)(Math.cos((input + 1) * Math.PI) / 2.0f) + 0.5f;
    }
}
```

![](https://ws2.sinaimg.cn/large/006tNbRwly1fvf4v86pcnj31go0omjve.jpg)
- 过载插值器   
$$y={(x-1)}^2(3(x-1)+2)+1$$
```
public class OvershootInterpolator extends BaseInterpolator implements NativeInterpolatorFactory {
    private final float mTension;

    public OvershootInterpolator() {
        mTension = 2.0f;
    }

    public OvershootInterpolator(float tension) {
        mTension = tension;
    }

    public float getInterpolation(float t) {
        // _o(t) = t * t * ((tension + 1) * t + tension)
        // o(t) = _o(t - 1) + 1
        t -= 1.0f;
        return t * t * ((mTension + 1) * t + mTension) + 1.0f;
    }
}
```

![](https://ws4.sinaimg.cn/large/006tNbRwly1fvf4v8n5udj31gg0osn0k.jpg)
 
### Android关键源码
```java
   Animation a = new TranslateAnimation(
       Animation.RELATIVE_TO_SELF, 0, 
       Animation.RELATIVE_TO_SELF, 1,
       Animation.RELATIVE_TO_SELF, 0,
       Animation.RELATIVE_TO_SELF, 1);

   View view = new View(this);
   view.startAnimation(a);
```
```java
    public void startAnimation(Animation animation) {
        animation.setStartTime(Animation.START_ON_FIRST_FRAME);
        setAnimation(animation);
        invalidateParentCaches();
        invalidate(true);
    }
```
```java
    public void setAnimation(Animation animation) {
        mCurrentAnimation = animation;
        ...
    }
```
```java
    public Animation getAnimation() {
        return mCurrentAnimation;
    }
```
```java
 /**
     * This method is called by ViewGroup.drawChild() to have each child view draw itself.
     *
     * This is where the View specializes rendering behavior based on layer type,
     * and hardware acceleration.
     */
    boolean draw(Canvas canvas, ViewGroup parent, long drawingTime) {
        ...

        Transformation transformToApply = null;
        final Animation a = getAnimation();
        // 改变了parent.getChildTransformation()
        more = applyLegacyAnimation(parent, drawingTime, a, scalingRequired);
        transformToApply = parent.getChildTransformation();
        
        ...

        int sx = 0;
        int sy = 0;
        if (!drawingWithRenderNode) {
            computeScroll();
            sx = mScrollX;
            sy = mScrollY;
        }

        final boolean offsetForScroll = cache == null && !drawingWithRenderNode;

        if (offsetForScroll) {
            canvas.translate(mLeft - sx, mTop - sy);
        } else {
            if (!drawingWithRenderNode) {
                canvas.translate(mLeft, mTop);
            }
            if (scalingRequired) {
                if (drawingWithRenderNode) {
                    // TODO: Might not need this if we put everything inside the DL
                    restoreTo = canvas.save();
                }
                // mAttachInfo cannot be null, otherwise scalingRequired == false
                final float scale = 1.0f / mAttachInfo.mApplicationScale;
                canvas.scale(scale, scale);
            }
        }
        
        ...

        if (transformToApply != null
                || alpha < 1
                || !hasIdentityMatrix()
                || (mPrivateFlags3 & PFLAG3_VIEW_IS_ANIMATING_ALPHA) != 0) {
            if (transformToApply != null || !childHasIdentityMatrix) {
                int transX = 0;
                int transY = 0;

                if (offsetForScroll) {
                    transX = -sx;
                    transY = -sy;
                }

                if (transformToApply != null) {
                    if (concatMatrix) {
                        if (drawingWithRenderNode) {
                            renderNode.setAnimationMatrix(transformToApply.getMatrix());
                        } else {
                            // Undo the scroll translation, apply the transformation matrix,
                            // then redo the scroll translate to get the correct result.
                            canvas.translate(-transX, -transY);
                            canvas.concat(transformToApply.getMatrix());
                            canvas.translate(transX, transY);
                        }
                        parent.mGroupFlags |= ViewGroup.FLAG_CLEAR_TRANSFORMATION;
                    }

                    ...
                }

                // ???
                if (!childHasIdentityMatrix && !drawingWithRenderNode) {
                    canvas.translate(-transX, -transY);
                    canvas.concat(getMatrix());
                    canvas.translate(transX, transY);
                }
            }

            ...
    }

```
```java
private boolean applyLegacyAnimation(ViewGroup parent, long drawingTime,
            Animation a, boolean scalingRequired) {
        Transformation invalidationTransform;
        final int flags = parent.mGroupFlags;
        final boolean initialized = a.isInitialized();
        if (!initialized) {
            a.initialize(mRight - mLeft, mBottom - mTop, parent.getWidth(), parent.getHeight());
            a.initializeInvalidateRegion(0, 0, mRight - mLeft, mBottom - mTop);
            if (mAttachInfo != null) a.setListenerHandler(mAttachInfo.mHandler);
            onAnimationStart();
        }

        final Transformation t = parent.getChildTransformation();
        boolean more = a.getTransformation(drawingTime, t, 1f);
        if (scalingRequired && mAttachInfo.mApplicationScale != 1f) {
            if (parent.mInvalidationTransformation == null) {
                parent.mInvalidationTransformation = new Transformation();
            }
            invalidationTransform = parent.mInvalidationTransformation;
            a.getTransformation(drawingTime, invalidationTransform, 1f);
        } else {
            invalidationTransform = t;
        }

        ...

        return more;
    }

```
```java
    public boolean getTransformation(long currentTime, Transformation outTransformation,
            float scale) {
        mScaleFactor = scale;
        return getTransformation(currentTime, outTransformation);
    }
```


## iOS(SDK 11.4)  
- UIView.  
```objectivec
@interface UIView(UIViewGeometry)

// animatable. do not use frame if view is transformed since it will not correctly reflect the actual location of the view. use bounds + center instead.
@property(nonatomic) CGRect            frame;

// use bounds/center and not frame if non-identity transform. if bounds dimension is odd, center may be have fractional part
@property(nonatomic) CGRect            bounds;      // default bounds is zero origin, frame size. animatable
@property(nonatomic) CGPoint           center;      // center is center of frame. animatable
@property(nonatomic) CGAffineTransform transform;   // default is CGAffineTransformIdentity. animatable
@property(nonatomic) CGFloat           contentScaleFactor NS_AVAILABLE_IOS(4_0);
....
@end
```

- [UIView animateWith***:....].  
```objectivec
@interface UIView(UIViewAnimationWithBlocks)
+ (void)animateWithDuration:(NSTimeInterval)duration 
        delay:(NSTimeInterval)delay 
        options:(UIViewAnimationOptions)options 
        animations:(void (^)(void))animations 
        completion:(void (^ __nullable)(BOOL finished))completion NS_AVAILABLE_IOS(4_0);
@end
```

- CGAffine*. 
```objectivec
struct CGAffineTransform {
  CGFloat a, b, c, d;
  CGFloat tx, ty;
};

/* The identity transform: [ 1 0 0 1 0 0 ]. */

CG_EXTERN const CGAffineTransform CGAffineTransformIdentity
  CG_AVAILABLE_STARTING(__MAC_10_0, __IPHONE_2_0);

/* Return the transform [ a b c d tx ty ]. */

CG_EXTERN CGAffineTransform CGAffineTransformMake(CGFloat a, CGFloat b,
  CGFloat c, CGFloat d, CGFloat tx, CGFloat ty)
  CG_AVAILABLE_STARTING(__MAC_10_0, __IPHONE_2_0);

/* Return a transform which translates by `(tx, ty)':
     t' = [ 1 0 0 1 tx ty ] */

CG_EXTERN CGAffineTransform CGAffineTransformMakeTranslation(CGFloat tx,
  CGFloat ty) CG_AVAILABLE_STARTING(__MAC_10_0, __IPHONE_2_0);

/* Return a transform which scales by `(sx, sy)':
     t' = [ sx 0 0 sy 0 0 ] */

CG_EXTERN CGAffineTransform CGAffineTransformMakeScale(CGFloat sx, CGFloat sy)
  CG_AVAILABLE_STARTING(__MAC_10_0, __IPHONE_2_0);

/* Return a transform which rotates by `angle' radians:
     t' = [ cos(angle) sin(angle) -sin(angle) cos(angle) 0 0 ] */

CG_EXTERN CGAffineTransform CGAffineTransformMakeRotation(CGFloat angle)
  CG_AVAILABLE_STARTING(__MAC_10_0, __IPHONE_2_0);

/* Return true if `t' is the identity transform, false otherwise. */

CG_EXTERN bool CGAffineTransformIsIdentity(CGAffineTransform t)
  CG_AVAILABLE_STARTING(__MAC_10_4, __IPHONE_2_0);

/* Translate `t' by `(tx, ty)' and return the result:
     t' = [ 1 0 0 1 tx ty ] * t */

CG_EXTERN CGAffineTransform CGAffineTransformTranslate(CGAffineTransform t,
  CGFloat tx, CGFloat ty) CG_AVAILABLE_STARTING(__MAC_10_0, __IPHONE_2_0);

/* Scale `t' by `(sx, sy)' and return the result:
     t' = [ sx 0 0 sy 0 0 ] * t */

CG_EXTERN CGAffineTransform CGAffineTransformScale(CGAffineTransform t,
  CGFloat sx, CGFloat sy) CG_AVAILABLE_STARTING(__MAC_10_0, __IPHONE_2_0);

/* Rotate `t' by `angle' radians and return the result:
     t' =  [ cos(angle) sin(angle) -sin(angle) cos(angle) 0 0 ] * t */

CG_EXTERN CGAffineTransform CGAffineTransformRotate(CGAffineTransform t,
  CGFloat angle) CG_AVAILABLE_STARTING(__MAC_10_0, __IPHONE_2_0);

/* Concatenate `t2' to `t1' and return the result:
     t' = t1 * t2 */

CG_EXTERN CGAffineTransform CGAffineTransformConcat(CGAffineTransform t1,
  CGAffineTransform t2) CG_AVAILABLE_STARTING(__MAC_10_0, __IPHONE_2_0);
```

    
# 参考
- [机器人基础](https://item.jd.com/29423361835.html) 
- [Learn OpenGL](https://learnopengl-cn.github.io/01%20Getting%20started/07%20Transformations/) 
- [Animation源码分析](http://www.10tiao.com/html/227/201803/2650242435/1.html)
- [理解矩阵乘法](https://www.cnblogs.com/alantu2018/p/8528299.html)
- [理解矩阵](https://blog.csdn.net/myan/article/details/647511)
- [Android Matrix详解](https://juejin.im/entry/59e8136af265da433226aea4)
- [matplotlib](https://github.com/matplotlib/matplotlib)