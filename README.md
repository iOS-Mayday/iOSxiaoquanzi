# iOS 面试败北感悟

#### 日常扯淡

去年`7月`, 第一次面试大公司: `饿了么`, 收到大公司的召唤非常的兴奋, 觉得自己翻身的机会终于要来了, 兴冲冲的跑去面试, 以为会和一般初级`iOS`面试的题目相同, 没有做任何的准备, 其实也不知道准备什么, 记得那时候聊的是:

1.  UI方面: `如何避免卡顿掉帧`, `异步渲染`.
2.  性能方面: `性能优化`, `Vsync`, `CPU / GPU`
3.  网络方面: 如何进行请求`缓存策略`.
4.  安全方面: `lild重签名`, `Mach-O`.
5.  前端方面: 如何避免`DOM`重绘.
6.  后端方面: 如何进行`负载均衡`的处理.

还有一些极端的情况, 由于时间久远已经记不太清了, 反正这次面试给我的感觉就是, 靠... 我简直就是个垃圾啊~ 当时记得内推我的架构师建议我扎实一下`iOS的基本功`, 然后就推荐了基本书: `计算机网络`, `操作系统原理`, `HTTP权威指南`, `TCP/IP权威指南`, `深入解析Max OS X & iOS 操作系统`... 这些书, 除了`HTTP权威指南`我咬着牙看完了, 其他的对于我来说简直就是天书, 根本消化不良啊.

去年`12月`, 第二次面试大公司: `京东`, 由于有了上一次的经验, 我变得非常的淡定, 知道自己肯定会被大公司所淘汰, 和`优等专业生`有着`不可逾越`的`天堑`, 比较吃惊的是, 进入京东的大楼需要用`身份证`换取`临时门禁`... 那时候的面试题就比饿了么的柔和的多了, 虽然当时还是回答不出.

1.  Runtime: `isa`, `消息转发`, `弱引用表`.
2.  Runloop: `mode`, `timer`.
3.  Block: `__block`, `__forwording`.
4.  Property: `assign`, `weak`, `copy`.
5.  Category: `assoc`, `load`

现在想想, 这TM才是面试`iOS`啊, 只可惜, 那时候并不具备这些知识, 可惜了了. 之后我就对`C++`, `ASM`, `Linux`, 这三方面进行了学习, 也学习了些`MACH-O`, `逆向`的相关的知识, 以备后面的面试机会.

第三次就是`这周三`面试`第二梯队`了, 我准备了所有我能够准备的面试题内容, `底层原理`, `逆向安全`, `多线程与锁`, `内存布局`, `UI性能优化`等, 果不其然, 足足`1个小时`的电话面试, 我轻松通过, 问的就是些我准备好的`底层知识`, 让我觉得机会终于来了, 可是.... 当我现场面试后... 题目全部是上机题...

1.  设计一个网络框架, 如何进行不同数据解析的设计(header, body), 并能够进行自定义, 重连机制如何处理, 状态码错误转发机制的处理, 如何避免回调地狱, 实现`Promise`的自实现.
2.  根据`UIControl`实现`UIButton`....
3.  找到两个排序数组的中位数...
4.  pow(double, double)函数的自实现....

果然,`网络`, `UI`, `算法`... 好吧, 第一次做上机题, 瞬间就蒙了... 然后就是面试官在旁边不停的笑... 不停的笑... 可能是他对我友好的一种方式吧... 最后的面试结论是, 我的`知识面`还是`比较广`的, `做过的东西`也是`挺多`的, 但是在`知识深度`方面还是比较欠缺.

后来得知, 他们招聘的是`技术专家`的职级, 觉得我的技术水平达不到要求, 不能给与录取, 当然被拒我是当场就知道了, 也觉得`美团`和`饿了么`的两次面试经历都和我的水平相差甚远... 可是, 我只是想进入大公司学习, 就一定要`成为专家`才行么? 现在`初中高级`, `资深`都不需要了, 直接专家么..., 我的猎头朋友和我说, `3-1`的级别就是对标阿里`P6/P7`, 我选择去死啊.....

有了这次的面试经验后, 我的策略也发生了改变, 不再追求大公司的光环了, 做人`开心一点`不好么, 非要`像机器一样思考`? `活的像个算法`一样是不是感觉缺少了些重要的东西? 和朋友们`吹吹牛逼`的日子不快活么, 偏要用功学习? 这里想到了最近听到的两句话, `最好的产品体验就是要让用户不用思考`, `认知即痛苦, 无知即极乐`, 人真是矛盾, 价值观的一念之差, 差之毫厘失之千里啊....

#### 面试题1 自实现pow(double, double)

这道题目上机的时候非常的蒙, 因为幂是`double`, 完全不知道如何下手, 面试官就降低难度使用整型.

##### 解法1

```
func _pow_1(_ base: Int, _ exponent: Int) -> Int {

    if exponent < 0 {
        return 0
    }
    if exponent == 0 {
        return 0
    }
    if exponent == 1 {
        return base
    }

    var result = base
    for _ in 1..<exponent {
        result *= base
    }
    return result
}
```

然后, 第一次做算法题的我, 只能想到通过最为粗糙的办法解答, 就是一个循环, 我也知道这不是面试官所期许的答案, 但这有什么办法呢...

##### 解法2

```
func _pow_2(_ base: Double, _ exponent: Int) -> Double {

    if exponent < 0 {
        return 0
    }
    if exponent == 0 {
        return 0
    }
    var ans:Double = 1, last_pow = base, exp = exponent

    while exp > 0 {
        if (exp & 1) != 0 {
            ans = ans * last_pow
        }
        exp = exp >> 1
        last_pow = last_pow * last_pow
    }
    return ans
}
```

这个是我在网上翻阅资料后的另一种看似比较好的解答方式.

##### 解法3

```
func _pow_3(_ base: Double, _ exponent: Int) -> Double {

    var isNegative = false
    var exp = exponent
    if exp < 0 {
        isNegative = true
        exp = -exp
    }
    let result = _pow_2(base, exp)
    return isNegative ? 1 / result : result
}

```

这个仅仅是加了一个负值判断.... 但是幂是`double`的仍然是毫无头绪, 需要请大佬和大神`不吝赐教`.

> 如果你正在跳槽或者正准备跳槽不妨动动小手，添加一下咱们的交流群[**1012951431**](https://links.jianshu.com/go?to=https%3A%2F%2Fjq.qq.com%2F%3F_wv%3D1027%26k%3D5JFjujE)来获取一份详细的大厂面试资料为你的跳槽多添一份保障。

#### 面试题2 findMedianSortedArrays

这是一道LeetCode的原题, 但是我至今还没有刷过算法题库... 也是平生第一次面试的时候遇到算法题. 题目的意思是找到两个排序数组的中位数.

##### 解法1

```
func findMedianSortedArrays_1(_ array1: [Int], _ array2: [Int]) -> Double {

    var array = [Int]()
    array.append(contentsOf: array1)
    array.append(contentsOf: array2)

    quickSort(list: &array)

    let b = array.count % 2
    let c = array.count
    var result = 0.0;
    if  b == 1  {
        result = Double(array[c / 2])
    } else {
        let n1 = array[c / 2 - 1]
        let n2 = array[c / 2]
        result = Double((n1 + n2)) / 2.0
    }

    return result
}
```

第一次做算法题, 只能无视算法复杂度, 能够完成就算是不错了, 要什么自行车...

##### 解法2

```
func findMedianSortedArrays_2(_ array1: [Int], _ array2: [Int]) -> Double {

    let c1 = array1.count, c2 = array2.count
    var a1 = array1, a2 = array2
    if c1 <= 0 && c2 <= 0 {
        return 0.0
    }

    func findKth(_ nums1: inout [Int], i: Int, _ nums2: inout [Int], j: Int, k: Int) -> Double {
        if nums1.count - i > nums2.count - j {
            return findKth(&nums2, i: j, &nums1, j: i, k: k)
        }
        if nums1.count == i {
            return Double(nums2[j + k - 1])
        }
        if k == 1 {
            return Double(min(nums1[i], nums2[j]))
        }
        let pa = min(i + k / 2, nums1.count), pb = j + k - pa + i
        if nums1[pa - 1] < nums2[pb - 1] {
            return findKth(&nums1, i: pa, &nums2, j: j, k: k - pa + i)
        } else if nums1[pa - 1] > nums2[pb - 1] {
            return findKth(&nums1, i: i, &nums2, j: pb, k: k - pb + j)
        } else {
            return Double(nums1[pa - 1])
        }
    }

    let total = c1 + c2
    if total % 2 == 1 {
        return findKth(&a1, i: 0, &a2, j: 0, k: total / 2 + 1)
    } else {
        return (findKth(&a1, i: 0, &a2, j: 0, k: total / 2) + findKth(&a1, i: 0, &a2, j: 0, k: total / 2 + 1)) / 2.0
    }
}
```

这个是我在网上查资料的时候的答案... 还没理清是个什么思路, 反正面试官提示分而治之, 掐头去尾... 也不知道是不是最优算法.

##### 解法3

```
func findMedianSortedArrays_3(_ array1: [Int], _ array2: [Int]) -> Double {

    let total = array1.count + array2.count
    let index = total / 2
    let count = array1.count < array2.count ? array2.count : array1.count
    var array = [Int]()

    var i = 0, j = 0;
    for _ in 0...count {
        if array.count >= index + 1 {
            break
        }
        if array1[i] < array2[j] {
            array.append(array1[i])
            i += 1
        } else {
            array.append(array2[j])
            j += 1
        }
    }
    return total % 2 == 1 ? Double(array[index]) : Double(array[index] + array[index - 1]) * 0.5
}
```

这个是请教霜神([@halfrost-一缕殇流化隐半边冰霜](https://github.com/halfrost))后给的思路, 的确很好实现. 但霜神谦虚的说不是最优解....

##### 奇数测试

```
var array1 = randomList(1000001)
var array2 = randomList(1000000)
quickSort(list: &array1)
quickSort(list: &array2)
print(findMedianSortedArrays_1(array1, array2))
print(findMedianSortedArrays_2(array1, array2))
print(findMedianSortedArrays_3(array1, array2))
```

```
--- scope of: findMedianSortedArrays ---
500045.0
500045.0
500045.0
```

##### 偶数测试

```
var array1 = randomList(1000001)
var array2 = randomList(1000000)
quickSort(list: &array1)
quickSort(list: &array2)
print(findMedianSortedArrays_1(array1, array2))
print(findMedianSortedArrays_2(array1, array2))
print(findMedianSortedArrays_3(array1, array2))
```

```
--- scope of: findMedianSortedArrays ---
499665.5
499665.5
499665.5
```

##### 耗时比较

```
--- scope of: findMedianSortedArrays_1 ---
timing: 2.50845623016357
--- scope of: findMedianSortedArrays_2 ---
timing: 1.28746032714844e-05
--- scope of: findMedianSortedArrays_3 ---
timing: 0.0358490943908691
```

可以看出网上查资料的答案是三种解法里性能最高的算法, 霜神的思路和网上的答案差了三个数量级, 而我写的差了五个数量级.... 果然我写的果然是最为垃圾的算法....

##### 解法4

```
@discardableResult func findMedianSortedArrays_4(_ array1: [Int], _ array2: [Int]) -> Double {

    if array1.count == 0 {
        if array2.count % 2 == 1 {
            return Double(array2[array2.count / 2])
        } else {
            return Double(array2[array2.count / 2] + array2[array2.count / 2 - 1]) * 0.5
        }
    } else if array2.count == 0 {
        if array1.count % 2 == 1 {
            return Double(array1[array1.count / 2])
        } else {
            return Double(array1[array1.count / 2] + array1[array1.count / 2 - 1]) * 0.5
        }
    }

    let total = array1.count + array2.count
    let count = array1.count < array2.count ? array1.count : array2.count
    let odd = total % 2 == 1

    var i = 0, j = 0, f = 1, m1 = 0.0, m2 = 0.0, result = 0.0;
    for _ in 0...count {
        if odd { array1[i] < array2[j] ? (i += 1) : (j += 1) }
        if f >= total / 2 {
            if odd {
                result = array1[i] < array2[j] ? Double(array1[i]) : Double(array2[j])
            } else {
                if array1[i] < array2[j] {
                    m1 = Double(array1[i])
                    if (i + 1) < array1.count && array1[i + 1] < array2[j] {
                        m2 = Double(array1[i + 1])
                    } else {
                        m2 = Double(array2[j])
                    }
                } else {
                    m1 = Double(array2[j])
                    if (j + 1) < array2.count && array2[j + 1] < array1[i] {
                        m2 = Double(array2[j + 1])
                    } else {
                        m2 = Double(array1[i])
                    }
                }
                result = (m1 + m2) * 0.5
            }
            break
        }
        if !odd { array1[i] < array2[j] ? (i += 1) : (j += 1) }
        f += 1
    }
    return result
}
```

```
--- scope of: findMedianSortedArrays_3 ---
timing: 0.0358932018280029
--- scope of: findMedianSortedArrays_4 ---
timing: 0.0241639614105225
```

沿着霜神的思路和面试官给的提示, 给出了上面的算法, 但是`解法3`的数量级是相同的

> 如果你正在跳槽或者正准备跳槽不妨动动小手，添加一下咱们的交流群[**1012951431**](https://links.jianshu.com/go?to=https%3A%2F%2Fjq.qq.com%2F%3F_wv%3D1027%26k%3D5JFjujE)来获取一份详细的大厂面试资料为你的跳槽多添一份保障。

#### 面试题3 UIContorl -> UIButton

```
protocol ButtonInterface {
    func setTitle(_ title: String);
    func setTitleColor(_ titleColor: UIColor);
    func setTitleEdgeInsets(_ edgeInsets: UIEdgeInsets);
    func setImage(_ image: UIImage);
    func setBackgroundImage(_ image: UIImage);
    func setImageEdgeInsets(_ edgeInsets: UIEdgeInsets);
}

class Button: UIControl, ButtonInterface { 

}
```

以上就是面试时候的原题, 一开始根本不知道是要让我做些什么, 说是只要让我把上面的方法全部实现就好了, 就像实现一个自己的UIButton... 一开始, 我以为是要我用`CALayer`来实现, 吓的我瑟瑟发抖... 还好不是... 然后, 我就按照自己平时自定义控件的写法, 写了一个`UIImageView`, 一个`UILabel`, 然后布局赋值... 就看到面试官对着我笑着说, 你以为这道题这么简单么... 这么简单么...

然后说, 你知道`UIButton setTitle`的时候才会创建`UILabel`, `setImage`的时候才会创建`UIImageView`, 你为什么吧`frame`给写死... 不知道`UIView`有`sizeToFit`么, 你怎么不实现`sizeThatFits`, 你是完全不会用吧... 你知道`UIButton`用`AutoLayout`布局的时候只要设置`origin`坐标, 宽高就可以自适应了, 你自定义的时候怎么不实现呢? `setBackgroundImage`和`setImageEdgeInsets`你就不要做了吧, 反正你也不会...

我想说的是,谁没事放着`UIButton`不用, 用`UIContorl`这种东西... 就为了一个`target-action`的设计模式么... 我每次在想思路的时候一直打断我, 可能这是面试官的一种策略吧... 算了不吐槽了, 还是尽力实现吧.

```
import UIKit

protocol ButtonInterface {
    func setTitle(_ title: String);
    func setTitleColor(_ titleColor: UIColor);
    func setTitleEdgeInsets(_ edgeInsets: UIEdgeInsets);
    func setImage(_ image: UIImage);
    func setBackgroundImage(_ image: UIImage);
    func setImageEdgeInsets(_ edgeInsets: UIEdgeInsets);
}

class Button: UIControl, ButtonInterface {

    lazy var titleLabel: UILabel = UILabel()
    lazy var imageView: UIImageView = UIImageView()
    lazy var backgroundImageView: UIImageView = UIImageView()

    var titleLabelIsCreated = false
    var imageViewIsCreated = false
    var backgroundImageViewCreated = false

    internal func setTitle(_ text: String) {
        if !titleLabelIsCreated {
            addSubview(titleLabel)
            titleLabelIsCreated = true
        }
        titleLabel.text = text
    }

    internal func setTitleColor(_ textColor: UIColor) {
        if !titleLabelIsCreated {
            return
        }
        titleLabel.textColor = textColor
    }

    internal func setTitleEdgeInsets(_ edgeInsets: UIEdgeInsets) {
        if !titleLabelIsCreated {
            return
        }
    }

    internal func setImage(_ image: UIImage) {
        if !imageViewIsCreated {
            addSubview(imageView)
            imageViewIsCreated = true
        }
        imageView.image = image
    }

    internal func setBackgroundImage(_ image: UIImage) {
        if !backgroundImageViewCreated {
            addSubview(backgroundImageView)
            insertSubview(backgroundImageView, at: 0)
            backgroundImageViewCreated = true
        }
        backgroundImageView.image = image
    }

    internal func setImageEdgeInsets(_ edgeInsets: UIEdgeInsets) {
        if !imageViewIsCreated {
            return
        }
    }

    override func sizeThatFits(_ size: CGSize) -> CGSize {
        if titleLabelIsCreated && !imageViewIsCreated && !backgroundImageViewCreated {
            let text: NSString? = titleLabel.text as NSString?
            let titleLabelW: CGFloat = text?.boundingRect(with: CGSize(width: CGFloat(MAXFLOAT), height: bounds.height), options: NSStringDrawingOptions.usesLineFragmentOrigin, attributes: [NSAttributedStringKey.font : titleLabel.font], context: nil).size.width ?? 0.0
            let titleLabelH: CGFloat = text?.boundingRect(with: CGSize(width: titleLabelW, height: CGFloat(MAXFLOAT)), options: NSStringDrawingOptions.usesLineFragmentOrigin, attributes: [NSAttributedStringKey.font : titleLabel.font], context: nil).size.height ?? 0.0
            return CGSize(width: titleLabelW, height: titleLabelH + 10)
        } else if !titleLabelIsCreated && imageViewIsCreated {
            return imageView.image?.size ?? CGSize.zero
        } else if titleLabelIsCreated && imageViewIsCreated {
            let text: NSString? = titleLabel.text as NSString?
            let titleLabelW: CGFloat = text?.boundingRect(with: CGSize(width: CGFloat(MAXFLOAT), height: bounds.height), options: NSStringDrawingOptions.usesLineFragmentOrigin, attributes: [NSAttributedStringKey.font : titleLabel.font], context: nil).size.width ?? 0.0
            let titleLabelH: CGFloat = text?.boundingRect(with: CGSize(width: titleLabelW, height: CGFloat(MAXFLOAT)), options: NSStringDrawingOptions.usesLineFragmentOrigin, attributes: [NSAttributedStringKey.font : titleLabel.font], context: nil).size.height ?? 0.0
            let imageViewW: CGFloat = imageView.image?.size.width ?? 0.0
            let imageViewH: CGFloat = imageView.image?.size.height ?? 0.0
            return CGSize(width: titleLabelW + imageViewW, height: imageViewH > titleLabelH ? imageViewH : titleLabelH)
        } else {
            return backgroundImageView.image?.size ?? CGSize.zero
        }
    }

    override func layoutSubviews() {
        super.layoutSubviews()

        if titleLabelIsCreated && !imageViewIsCreated {
            titleLabel.frame = bounds
            titleLabel.textAlignment = .center
        } else if !titleLabelIsCreated && imageViewIsCreated {
            let y: CGFloat = 0;
            let width: CGFloat = imageView.image?.size.width ?? 0;
            let x: CGFloat = (bounds.width - width) * 0.5;
            let height: CGFloat = bounds.height;
            imageView.frame = CGRect(x: x, y: y, width: width, height: height)
        } else if titleLabelIsCreated && imageViewIsCreated {
            let imageViewY: CGFloat = 0;
            let imageViewW: CGFloat = imageView.image?.size.width ?? 0;
            let imageViewH: CGFloat = bounds.height;
            let text: NSString? = titleLabel.text as NSString?
            let titleLabelW: CGFloat = text?.boundingRect(with: CGSize(width: CGFloat(MAXFLOAT), height: bounds.height), options: NSStringDrawingOptions.usesLineFragmentOrigin, attributes: [NSAttributedStringKey.font : titleLabel.font], context: nil).size.width ?? 0.0
            let titleLabelH: CGFloat = text?.boundingRect(with: CGSize(width: titleLabelW, height: CGFloat(MAXFLOAT)), options: NSStringDrawingOptions.usesLineFragmentOrigin, attributes: [NSAttributedStringKey.font : titleLabel.font], context: nil).size.height ?? 0.0
            let imageViewX: CGFloat = (bounds.width - imageViewW - titleLabelW) * 0.5;
            let titleLabelX: CGFloat = imageViewX + imageViewW
            let titleLabelY = (bounds.height - titleLabelH) * 0.5
            titleLabel.frame = CGRect(x: titleLabelX, y: titleLabelY, width: titleLabelW, height: titleLabelH)
            imageView.frame = CGRect(x: imageViewX, y: imageViewY, width: imageViewW, height: imageViewH)
        }

        if backgroundImageViewCreated {
            backgroundImageView.frame = bounds
        }
    }
}
```

虽然实现了部分的功能, 但是`AutoLayout`和`EdgeInsets`的功能还是没有思路, 还请各位大佬解惑.

> 如果你正在跳槽或者正准备跳槽不妨动动小手，添加一下咱们的交流群[**1012951431**](https://links.jianshu.com/go?to=https%3A%2F%2Fjq.qq.com%2F%3F_wv%3D1027%26k%3D5JFjujE)来获取一份详细的大厂面试资料为你的跳槽多添一份保障。

##### 测试对比

我们用自己自实现的`Button`和`UIButton`进行对比.

```
class ViewController: UIViewController {

    override func loadView() {
        super.loadView();

        let uibutton = UIButton()
        uibutton.layer.borderWidth = 1
        uibutton.layer.borderColor = UIColor.black.cgColor

        uibutton.frame = CGRect(x: 0, y: 100, width: view.bounds.width, height: 40)
        uibutton.setTitle("github.com/coderZsq", for: .normal)
        uibutton.setTitleColor(.red, for: .normal)
        uibutton.titleEdgeInsets = UIEdgeInsetsMake(0, 0, 0, 0)
        uibutton.setImage(UIImage(named: "avatar"), for: .normal)
        uibutton.setBackgroundImage(UIImage(named: "avatar"), for: .normal)
        uibutton.imageEdgeInsets = UIEdgeInsetsMake(-10, -10, -10, -10)
        view.addSubview(uibutton)
        uibutton.sizeToFit()

        let button = Button()
        button.layer.borderWidth = 1
        button.layer.borderColor = UIColor.black.cgColor

        button.frame = CGRect(x: 0, y: 350, width: view.bounds.width, height: 40)
        button.setTitle("github.com/coderZsq")
        button.setTitleColor(.red)
        button.setTitleEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        button.setImage(UIImage(named: "avatar") ?? UIImage());
        button.setBackgroundImage(UIImage(named: "avatar") ?? UIImage())
        button.setImageEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        view.addSubview(button)
        button.sizeToFit()
    }
}
```

##### zero impl

```

        let uibutton = UIButton()
        uibutton.layer.borderWidth = 1
        uibutton.layer.borderColor = UIColor.black.cgColor

        uibutton.frame = CGRect(x: 0, y: 100, width: view.bounds.width, height: 40)
//        uibutton.setTitle("github.com/coderZsq", for: .normal)
//        uibutton.setTitleColor(.red, for: .normal)
//        uibutton.titleEdgeInsets = UIEdgeInsetsMake(0, 0, 0, 0)
//        uibutton.setImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.setBackgroundImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.imageEdgeInsets = UIEdgeInsetsMake(-10, -10, -10, -10)
        view.addSubview(uibutton)
//        uibutton.sizeToFit()

        let button = Button()
        button.layer.borderWidth = 1
        button.layer.borderColor = UIColor.black.cgColor

        button.frame = CGRect(x: 0, y: 350, width: view.bounds.width, height: 40)
//        button.setTitle("github.com/coderZsq")
//        button.setTitleColor(.red)
//        button.setTitleEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
//        button.setImage(UIImage(named: "avatar") ?? UIImage());
//        button.setBackgroundImage(UIImage(named: "avatar") ?? UIImage())
//        button.setImageEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        view.addSubview(button)
//        button.sizeToFit()

```

![](https://upload-images.jianshu.io/upload_images/22877992-c4091557eb1c1910.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](https://upload-images.jianshu.io/upload_images/22877992-5b06511656634dbf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



##### setTitle && setTitleColor

```
        let uibutton = UIButton()
        uibutton.layer.borderWidth = 1
        uibutton.layer.borderColor = UIColor.black.cgColor

        uibutton.frame = CGRect(x: 0, y: 100, width: view.bounds.width, height: 40)
        uibutton.setTitle("github.com/coderZsq", for: .normal)
        uibutton.setTitleColor(.red, for: .normal)
//        uibutton.titleEdgeInsets = UIEdgeInsetsMake(0, 0, 0, 0)
//        uibutton.setImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.setBackgroundImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.imageEdgeInsets = UIEdgeInsetsMake(-10, -10, -10, -10)
        view.addSubview(uibutton)
//        uibutton.sizeToFit()

        let button = Button()
        button.layer.borderWidth = 1
        button.layer.borderColor = UIColor.black.cgColor

        button.frame = CGRect(x: 0, y: 350, width: view.bounds.width, height: 40)
        button.setTitle("github.com/coderZsq")
        button.setTitleColor(.red)
//        button.setTitleEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
//        button.setImage(UIImage(named: "avatar") ?? UIImage());
//        button.setBackgroundImage(UIImage(named: "avatar") ?? UIImage())
//        button.setImageEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        view.addSubview(button)
//        button.sizeToFit()
    }
```

![](https://upload-images.jianshu.io/upload_images/22877992-b063bf91449b5e06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](https://upload-images.jianshu.io/upload_images/22877992-c8be0d42a8798b64.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### setTitle && setTitleColor && sizeToFit

```
        let uibutton = UIButton()
        uibutton.layer.borderWidth = 1
        uibutton.layer.borderColor = UIColor.black.cgColor

        uibutton.frame = CGRect(x: 0, y: 100, width: view.bounds.width, height: 40)
        uibutton.setTitle("github.com/coderZsq", for: .normal)
        uibutton.setTitleColor(.red, for: .normal)
//        uibutton.titleEdgeInsets = UIEdgeInsetsMake(0, 0, 0, 0)
//        uibutton.setImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.setBackgroundImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.imageEdgeInsets = UIEdgeInsetsMake(-10, -10, -10, -10)
        view.addSubview(uibutton)
        uibutton.sizeToFit()

        let button = Button()
        button.layer.borderWidth = 1
        button.layer.borderColor = UIColor.black.cgColor

        button.frame = CGRect(x: 0, y: 350, width: view.bounds.width, height: 40)
        button.setTitle("github.com/coderZsq")
        button.setTitleColor(.red)
//        button.setTitleEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
//        button.setImage(UIImage(named: "avatar") ?? UIImage());
//        button.setBackgroundImage(UIImage(named: "avatar") ?? UIImage())
//        button.setImageEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        view.addSubview(button)
        button.sizeToFit()
```

![](https://upload-images.jianshu.io/upload_images/22877992-85b318b4d7eb3f2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](https://upload-images.jianshu.io/upload_images/22877992-e94298840d24f9a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### setImage

```
        let uibutton = UIButton()
        uibutton.layer.borderWidth = 1
        uibutton.layer.borderColor = UIColor.black.cgColor

        uibutton.frame = CGRect(x: 0, y: 100, width: view.bounds.width, height: 40)
//        uibutton.setTitle("github.com/coderZsq", for: .normal)
//        uibutton.setTitleColor(.red, for: .normal)
//        uibutton.titleEdgeInsets = UIEdgeInsetsMake(0, 0, 0, 0)
        uibutton.setImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.setBackgroundImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.imageEdgeInsets = UIEdgeInsetsMake(-10, -10, -10, -10)
        view.addSubview(uibutton)
//        uibutton.sizeToFit()

        let button = Button()
        button.layer.borderWidth = 1
        button.layer.borderColor = UIColor.black.cgColor

        button.frame = CGRect(x: 0, y: 350, width: view.bounds.width, height: 40)
//        button.setTitle("github.com/coderZsq")
//        button.setTitleColor(.red)
//        button.setTitleEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        button.setImage(UIImage(named: "avatar") ?? UIImage());
//        button.setBackgroundImage(UIImage(named: "avatar") ?? UIImage())
//        button.setImageEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        view.addSubview(button)
//        button.sizeToFit()

```

![](https://upload-images.jianshu.io/upload_images/22877992-a4b7bfdc96d04c8b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](https://upload-images.jianshu.io/upload_images/22877992-2b225f2e5112f874.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### setImage && sizeToFit

```
        let uibutton = UIButton()
        uibutton.layer.borderWidth = 1
        uibutton.layer.borderColor = UIColor.black.cgColor

        uibutton.frame = CGRect(x: 0, y: 100, width: view.bounds.width, height: 40)
//        uibutton.setTitle("github.com/coderZsq", for: .normal)
//        uibutton.setTitleColor(.red, for: .normal)
//        uibutton.titleEdgeInsets = UIEdgeInsetsMake(0, 0, 0, 0)
        uibutton.setImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.setBackgroundImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.imageEdgeInsets = UIEdgeInsetsMake(-10, -10, -10, -10)
        view.addSubview(uibutton)
        uibutton.sizeToFit()

        let button = Button()
        button.layer.borderWidth = 1
        button.layer.borderColor = UIColor.black.cgColor

        button.frame = CGRect(x: 0, y: 350, width: view.bounds.width, height: 40)
//        button.setTitle("github.com/coderZsq")
//        button.setTitleColor(.red)
//        button.setTitleEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        button.setImage(UIImage(named: "avatar") ?? UIImage());
//        button.setBackgroundImage(UIImage(named: "avatar") ?? UIImage())
//        button.setImageEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        view.addSubview(button)
        button.sizeToFit()
```

![](https://upload-images.jianshu.io/upload_images/22877992-3e0f7a640525b923.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](https://upload-images.jianshu.io/upload_images/22877992-ad1b8b260e059154.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### setBackgroundImage

```
        let uibutton = UIButton()
        uibutton.layer.borderWidth = 1
        uibutton.layer.borderColor = UIColor.black.cgColor

        uibutton.frame = CGRect(x: 0, y: 100, width: view.bounds.width, height: 40)
//        uibutton.setTitle("github.com/coderZsq", for: .normal)
//        uibutton.setTitleColor(.red, for: .normal)
//        uibutton.titleEdgeInsets = UIEdgeInsetsMake(0, 0, 0, 0)
//        uibutton.setImage(UIImage(named: "avatar"), for: .normal)
        uibutton.setBackgroundImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.imageEdgeInsets = UIEdgeInsetsMake(-10, -10, -10, -10)
        view.addSubview(uibutton)
//        uibutton.sizeToFit()

        let button = Button()
        button.layer.borderWidth = 1
        button.layer.borderColor = UIColor.black.cgColor

        button.frame = CGRect(x: 0, y: 350, width: view.bounds.width, height: 40)
//        button.setTitle("github.com/coderZsq")
//        button.setTitleColor(.red)
//        button.setTitleEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
//        button.setImage(UIImage(named: "avatar") ?? UIImage());
        button.setBackgroundImage(UIImage(named: "avatar") ?? UIImage())
//        button.setImageEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        view.addSubview(button)
//        button.sizeToFit()

```

![](https://upload-images.jianshu.io/upload_images/22877992-e3ddd4805418264b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/22877992-f4e709bf09b1e8fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这里, 我们看到和系统的实现不一样, 因为我直接在渲染(`display`)设置了`bitmap`, 而不是像系统添加了一个新的`view`. 这样的性能消耗会少些... (以修改 为了下面的问题)

> 如果你正在跳槽或者正准备跳槽不妨动动小手，添加一下咱们的交流群[**1012951431**](https://links.jianshu.com/go?to=https%3A%2F%2Fjq.qq.com%2F%3F_wv%3D1027%26k%3D5JFjujE)来获取一份详细的大厂面试资料为你的跳槽多添一份保障。

##### setBackgroundImage && sizeToFit

```
        let uibutton = UIButton()
        uibutton.layer.borderWidth = 1
        uibutton.layer.borderColor = UIColor.black.cgColor

        uibutton.frame = CGRect(x: 0, y: 100, width: view.bounds.width, height: 40)
//        uibutton.setTitle("github.com/coderZsq", for: .normal)
//        uibutton.setTitleColor(.red, for: .normal)
//        uibutton.titleEdgeInsets = UIEdgeInsetsMake(0, 0, 0, 0)
//        uibutton.setImage(UIImage(named: "avatar"), for: .normal)
        uibutton.setBackgroundImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.imageEdgeInsets = UIEdgeInsetsMake(-10, -10, -10, -10)
        view.addSubview(uibutton)
        uibutton.sizeToFit()

        let button = Button()
        button.layer.borderWidth = 1
        button.layer.borderColor = UIColor.black.cgColor

        button.frame = CGRect(x: 0, y: 350, width: view.bounds.width, height: 40)
//        button.setTitle("github.com/coderZsq")
//        button.setTitleColor(.red)
//        button.setTitleEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
//        button.setImage(UIImage(named: "avatar") ?? UIImage());
        button.setBackgroundImage(UIImage(named: "avatar") ?? UIImage())
//        button.setImageEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        view.addSubview(button)
        button.sizeToFit()

```

![](https://upload-images.jianshu.io/upload_images/22877992-2c7635630ddf362e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](https://upload-images.jianshu.io/upload_images/22877992-bc5ebebbdc05fb54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### setTitle && setTitleColor && setImage

```
        let uibutton = UIButton()
        uibutton.layer.borderWidth = 1
        uibutton.layer.borderColor = UIColor.black.cgColor

        uibutton.frame = CGRect(x: 0, y: 100, width: view.bounds.width, height: 40)
        uibutton.setTitle("github.com/coderZsq", for: .normal)
        uibutton.setTitleColor(.red, for: .normal)
//        uibutton.titleEdgeInsets = UIEdgeInsetsMake(0, 0, 0, 0)
        uibutton.setImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.setBackgroundImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.imageEdgeInsets = UIEdgeInsetsMake(-10, -10, -10, -10)
        view.addSubview(uibutton)
//        uibutton.sizeToFit()

        let button = Button()
        button.layer.borderWidth = 1
        button.layer.borderColor = UIColor.black.cgColor

        button.frame = CGRect(x: 0, y: 350, width: view.bounds.width, height: 40)
        button.setTitle("github.com/coderZsq")
        button.setTitleColor(.red)
//        button.setTitleEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        button.setImage(UIImage(named: "avatar") ?? UIImage());
//        button.setBackgroundImage(UIImage(named: "avatar") ?? UIImage())
//        button.setImageEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        view.addSubview(button)
//        button.sizeToFit()


```

![](https://upload-images.jianshu.io/upload_images/22877992-dbb0277577e36aad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](https://upload-images.jianshu.io/upload_images/22877992-7fa7755a0e03c55b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### setTitle && setTitleColor && setImage && sizeToFit

```
        let uibutton = UIButton()
        uibutton.layer.borderWidth = 1
        uibutton.layer.borderColor = UIColor.black.cgColor

        uibutton.frame = CGRect(x: 0, y: 100, width: view.bounds.width, height: 40)
        uibutton.setTitle("github.com/coderZsq", for: .normal)
        uibutton.setTitleColor(.red, for: .normal)
//        uibutton.titleEdgeInsets = UIEdgeInsetsMake(0, 0, 0, 0)
        uibutton.setImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.setBackgroundImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.imageEdgeInsets = UIEdgeInsetsMake(-10, -10, -10, -10)
        view.addSubview(uibutton)
        uibutton.sizeToFit()

        let button = Button()
        button.layer.borderWidth = 1
        button.layer.borderColor = UIColor.black.cgColor

        button.frame = CGRect(x: 0, y: 350, width: view.bounds.width, height: 40)
        button.setTitle("github.com/coderZsq")
        button.setTitleColor(.red)
//        button.setTitleEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        button.setImage(UIImage(named: "avatar") ?? UIImage());
//        button.setBackgroundImage(UIImage(named: "avatar") ?? UIImage())
//        button.setImageEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        view.addSubview(button)
        button.sizeToFit()

```

![](https://upload-images.jianshu.io/upload_images/22877992-542c5cbdb40b9fdf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/22877992-b02098b5cd4332c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### setTitle && setTitleColor && setBackgroundImage

```

        let uibutton = UIButton()
        uibutton.layer.borderWidth = 1
        uibutton.layer.borderColor = UIColor.black.cgColor

        uibutton.frame = CGRect(x: 0, y: 100, width: view.bounds.width, height: 40)
        uibutton.setTitle("github.com/coderZsq", for: .normal)
        uibutton.setTitleColor(.red, for: .normal)
//        uibutton.titleEdgeInsets = UIEdgeInsetsMake(0, 0, 0, 0)
//        uibutton.setImage(UIImage(named: "avatar"), for: .normal)
        uibutton.setBackgroundImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.imageEdgeInsets = UIEdgeInsetsMake(-10, -10, -10, -10)
        view.addSubview(uibutton)
//        uibutton.sizeToFit()

        let button = Button()
        button.layer.borderWidth = 1
        button.layer.borderColor = UIColor.black.cgColor

        button.frame = CGRect(x: 0, y: 350, width: view.bounds.width, height: 40)
        button.setTitle("github.com/coderZsq")
        button.setTitleColor(.red)
//        button.setTitleEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
//        button.setImage(UIImage(named: "avatar") ?? UIImage());
        button.setBackgroundImage(UIImage(named: "avatar") ?? UIImage())
//        button.setImageEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        view.addSubview(button)
//        button.sizeToFit()
```

![](https://upload-images.jianshu.io/upload_images/22877992-de54d76f55a1fb14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](https://upload-images.jianshu.io/upload_images/22877992-93eaed1d8f393d73.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 如果你正在跳槽或者正准备跳槽不妨动动小手，添加一下咱们的交流群[**1012951431**](https://links.jianshu.com/go?to=https%3A%2F%2Fjq.qq.com%2F%3F_wv%3D1027%26k%3D5JFjujE)来获取一份详细的大厂面试资料为你的跳槽多添一份保障。

##### setTitle && setTitleColor && setBackgroundImage && sizeToFit

```
        let uibutton = UIButton()
        uibutton.layer.borderWidth = 1
        uibutton.layer.borderColor = UIColor.black.cgColor

        uibutton.frame = CGRect(x: 0, y: 100, width: view.bounds.width, height: 40)
        uibutton.setTitle("github.com/coderZsq", for: .normal)
        uibutton.setTitleColor(.red, for: .normal)
//        uibutton.titleEdgeInsets = UIEdgeInsetsMake(0, 0, 0, 0)
//        uibutton.setImage(UIImage(named: "avatar"), for: .normal)
        uibutton.setBackgroundImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.imageEdgeInsets = UIEdgeInsetsMake(-10, -10, -10, -10)
        view.addSubview(uibutton)
        uibutton.sizeToFit()

        let button = Button()
        button.layer.borderWidth = 1
        button.layer.borderColor = UIColor.black.cgColor

        button.frame = CGRect(x: 0, y: 350, width: view.bounds.width, height: 40)
        button.setTitle("github.com/coderZsq")
        button.setTitleColor(.red)
//        button.setTitleEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
//        button.setImage(UIImage(named: "avatar") ?? UIImage());
        button.setBackgroundImage(UIImage(named: "avatar") ?? UIImage())
//        button.setImageEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        view.addSubview(button)
        button.sizeToFit()

```

![](https://upload-images.jianshu.io/upload_images/22877992-e97850cd780dfa7d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![](https://upload-images.jianshu.io/upload_images/22877992-801e423578f7e650.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



##### setTitle && setTitleColor && setImage && setBackgroundImage

```
        let uibutton = UIButton()
        uibutton.layer.borderWidth = 1
        uibutton.layer.borderColor = UIColor.black.cgColor

        uibutton.frame = CGRect(x: 0, y: 100, width: view.bounds.width, height: 40)
        uibutton.setTitle("github.com/coderZsq", for: .normal)
        uibutton.setTitleColor(.red, for: .normal)
//        uibutton.titleEdgeInsets = UIEdgeInsetsMake(0, 0, 0, 0)
        uibutton.setImage(UIImage(named: "avatar"), for: .normal)
        uibutton.setBackgroundImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.imageEdgeInsets = UIEdgeInsetsMake(-10, -10, -10, -10)
        view.addSubview(uibutton)
//        uibutton.sizeToFit()

        let button = Button()
        button.layer.borderWidth = 1
        button.layer.borderColor = UIColor.black.cgColor

        button.frame = CGRect(x: 0, y: 350, width: view.bounds.width, height: 40)
        button.setTitle("github.com/coderZsq")
        button.setTitleColor(.red)
//        button.setTitleEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        button.setImage(UIImage(named: "avatar") ?? UIImage());
        button.setBackgroundImage(UIImage(named: "avatar") ?? UIImage())
//        button.setImageEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        view.addSubview(button)
//        button.sizeToFit()

```

![](https://upload-images.jianshu.io/upload_images/22877992-5591201092b070d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![](https://upload-images.jianshu.io/upload_images/22877992-cc7e21596a9afea1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### setTitle && setTitleColor && setImage && setBackgroundImage && sizeToFit

```
        let uibutton = UIButton()
        uibutton.layer.borderWidth = 1
        uibutton.layer.borderColor = UIColor.black.cgColor

        uibutton.frame = CGRect(x: 0, y: 100, width: view.bounds.width, height: 40)
        uibutton.setTitle("github.com/coderZsq", for: .normal)
        uibutton.setTitleColor(.red, for: .normal)
//        uibutton.titleEdgeInsets = UIEdgeInsetsMake(0, 0, 0, 0)
        uibutton.setImage(UIImage(named: "avatar"), for: .normal)
        uibutton.setBackgroundImage(UIImage(named: "avatar"), for: .normal)
//        uibutton.imageEdgeInsets = UIEdgeInsetsMake(-10, -10, -10, -10)
        view.addSubview(uibutton)
        uibutton.sizeToFit()

        let button = Button()
        button.layer.borderWidth = 1
        button.layer.borderColor = UIColor.black.cgColor

        button.frame = CGRect(x: 0, y: 350, width: view.bounds.width, height: 40)
        button.setTitle("github.com/coderZsq")
        button.setTitleColor(.red)
//        button.setTitleEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        button.setImage(UIImage(named: "avatar") ?? UIImage());
        button.setBackgroundImage(UIImage(named: "avatar") ?? UIImage())
//        button.setImageEdgeInsets(UIEdgeInsetsMake(0, 0, 0, 0))
        view.addSubview(button)
        button.sizeToFit()
```

![](https://upload-images.jianshu.io/upload_images/22877992-8cf35c67777920ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](https://upload-images.jianshu.io/upload_images/22877992-4ad4b2f0769287fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这道题还没有实现的就是`AutoLayout`和`EdgeInsets`, 还有就是在`sizeToFit`算的最佳尺寸和系统的最佳尺寸有细微出入, 还有就是`UIImageView`和`UILabel`的先后加载的问题.

##### update

```
    lazy var titleLabel: UILabel =  {
        let titleLabel = UILabel()
        titleLabel.font = UIFont.systemFont(ofSize: 18)
        titleLabel.textAlignment = .center
        return titleLabel
    }()

```

对于`在sizeToFit算的最佳尺寸和系统的最佳尺寸有细微出入` 这个问题是`UILabel`的默认系统字体大小是`17`, 而`UIButton`中的`titleLabel`的字体大小是`18`.

```
    internal func setImage(_ image: UIImage) {
        if !imageViewIsCreated {
            addSubview(imageView)
            if titleLabelIsCreated {
                insertSubview(imageView, belowSubview: titleLabel)
            }
            imageViewIsCreated = true
        }
        imageView.image = image
    }

```

对于`UIImageView和UILabel的先后加载的问题`, 需要在`setImage`时判断`titleLabel`是否存在即可

![](https://upload-images.jianshu.io/upload_images/22877992-558a18fd8d3090eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](https://upload-images.jianshu.io/upload_images/22877992-064cfc5d9aeae389.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


现已达到完全相同, 接下来就是要解决`AutoLayout`和`EdgeInsets`的问题.

##### update2

通过 nib 进行创建.. 谢谢`@梦痕_Lee`提供的思路.

![](https://upload-images.jianshu.io/upload_images/22877992-cb4c73a65e7236f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


实现方法:

1.判断是否是从nib中创建

```
    var createdFromNib = false

    override init(frame: CGRect) {
        super.init(frame: frame)
    }

    required init?(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)
        createdFromNib = true
    }

```

2.重写系统实现函数`intrinsicContentSize`并返回最佳尺寸

```
    override var intrinsicContentSize: CGSize {
        return bestSize()
    }

    func bestSize() -> CGSize {
        if titleLabelIsCreated && !imageViewIsCreated && !backgroundImageViewCreated {
            let text: NSString? = titleLabel.text as NSString?
            let titleLabelW: CGFloat = text?.boundingRect(with: CGSize(width: CGFloat(MAXFLOAT), height: bounds.height), options: NSStringDrawingOptions.usesLineFragmentOrigin, attributes: [NSAttributedStringKey.font : titleLabel.font], context: nil).size.width ?? 0.0
            let titleLabelH: CGFloat = text?.boundingRect(with: CGSize(width: titleLabelW, height: CGFloat(MAXFLOAT)), options: NSStringDrawingOptions.usesLineFragmentOrigin, attributes: [NSAttributedStringKey.font : titleLabel.font], context: nil).size.height ?? 0.0
            return CGSize(width: titleLabelW, height: titleLabelH + 10)
        } else if !titleLabelIsCreated && imageViewIsCreated {
            return imageView.image?.size ?? CGSize.zero
        } else if titleLabelIsCreated && imageViewIsCreated {
            let text: NSString? = titleLabel.text as NSString?
            let titleLabelW: CGFloat = text?.boundingRect(with: CGSize(width: CGFloat(MAXFLOAT), height: bounds.height), options: NSStringDrawingOptions.usesLineFragmentOrigin, attributes: [NSAttributedStringKey.font : titleLabel.font], context: nil).size.width ?? 0.0
            let titleLabelH: CGFloat = text?.boundingRect(with: CGSize(width: titleLabelW, height: CGFloat(MAXFLOAT)), options: NSStringDrawingOptions.usesLineFragmentOrigin, attributes: [NSAttributedStringKey.font : titleLabel.font], context: nil).size.height ?? 0.0
            let imageViewW: CGFloat = imageView.image?.size.width ?? 0.0
            let imageViewH: CGFloat = imageView.image?.size.height ?? 0.0
            return CGSize(width: titleLabelW + imageViewW, height: imageViewH > titleLabelH ? imageViewH : titleLabelH)
        } else {
            return backgroundImageView.image?.size ?? CGSize.zero
        }
    }

```

3.布局时判断即可

```
    override func layoutSubviews() {
        super.layoutSubviews()

        if createdFromNib {
            frame.size = intrinsicContentSize
        }
        ...
   }

```

4.测试一下, 不加sizetofit

```
        nib_button.layer.borderWidth = 1
        nib_button.layer.borderColor = UIColor.black.cgColor

        nib_button.setTitle("github.com/coderZsq")
        nib_button.setTitleColor(.red)
//        nib_button.setTitleEdgeInsets(.zero)
        nib_button.setImage(UIImage(named: "avatar") ?? UIImage())
//        nib_button.setBackgroundImage(UIImage(named: "avatar") ?? UIImage())
        nib_button.setImageEdgeInsets(.zero)

```

![](https://upload-images.jianshu.io/upload_images/22877992-51f40bed5a92e6f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


一样的完美, 攻克了`AutoLayout`, 接下来就是`EdgeInsets`啦~~~

##### update3

一直理解错了, `intrinsicContentSize`这个方法是在添加约束后生效, 不管是`nib`还是`手动约束`, 都会生效.

![](https://upload-images.jianshu.io/upload_images/22877992-937ca970c64132c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


所以`update2`中只需实现第二步即可, 并不需要其他的判断.

#### 面试题4 网络架构实现

这道题真是戳中我的软肋, 网络与多线程是我最为薄弱的地方, 现在对线程的理解应该已经不错了, 但是网络还是有所欠缺的, 所以, 我会在今后学习`Linux`和精读`AFNetWorking` && `SDWebImage`后, 自己写一个网络架构来进行自我提升.

#### 面试总结

对于两道算法题, 我没有什么太多想讲的, `技不如人吧`, 可是算法题这种东西在`iOS`上用处其实不是很大, 我就不信那些没有刷过算法题的同学, 面对一道从没有见过的算法题能够一下子`从容`的写出`最优解`的. 还有就是`UI`的那道题目, 坑吧, 谁会去放着好好的现成的不用, 去恶心自己, 还一定要一样... 我都问过面试官好几遍, 到底想要做什么功能... 回答只有, 我不在乎你怎么写, 只要和系统的一样就好了.... 产品附体了么....

好吧... 技不如人, 被挂了也是理所当然... 不会找什么借口... 听美团的朋友说, 现在只招技术专家, 其他低职级的名额紧缩都不招了... 诶... 随缘吧...

最后 本文中所有的源码都可以在github上找到:

> GitHub Repo：[coderZsq.target.swift](https://github.com/coderZsq/coderZsq.target.swift)
> Follow: [coderZsq · GitHub](https://github.com/coderZsq)
> Resume: [coderzsq.github.io/coderZsq.we…](https://coderzsq.github.io/coderZsq.webpack.js/#/)

# 资料推荐

如果你正在跳槽或者正准备跳槽不妨动动小手，添加一下咱们的交流群[**1012951431**](https://links.jianshu.com/go?to=https%3A%2F%2Fjq.qq.com%2F%3F_wv%3D1027%26k%3D5JFjujE)来获取一份详细的大厂面试资料为你的跳槽多添一份保障。

![](https://upload-images.jianshu.io/upload_images/22877992-0bfc037cc50cae7d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

