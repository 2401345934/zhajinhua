## 炸金花

```jsx
import React from 'react';

export default () => {
  //随机排序
  function randomsort(a, b) {
    return Math.random() > 0.5 ? -1 : 1;
    //用Math.random()函数生成0~1之间的随机数与0.5比较，返回-1或1
  }

  //生成有序的扑克牌并打乱顺序
  function getPorker() {
    var poker = []; //打乱后的扑克牌
    var orgPoker = []; //原始有序的扑克牌
    var num = [2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14];
    var color = ['♠', '❤', '♣', '♦'];
    //生成有序的扑克牌
    for (var i = 0; i < num.length; i++) {
      for (var j = 0; j < color.length; j++) {
        orgPoker.push(num[i] + color[j]);
      }
    }
    //将有序的扑克牌打乱，生成新的无序的扑克牌
    poker = orgPoker.sort(randomsort);
    //返回被打乱的扑克牌
    return poker;
  }

  //console.log(getPorker());

  //发牌，传进两个参数，玩家数量和洗好的牌，返回所有玩家的从打到小排序的手牌
  function deal(playerNum, poker) {
    var handPokers = []; //所有玩家的手牌的数组
    for (var i = 0; i < playerNum; i++) {
      var handPoker = []; //单个玩家的手牌
      handPoker.push(poker[i], poker[i + playerNum], poker[i + 2 * playerNum]);
      //讲单个玩家的手牌按照降序排列，然后放到所有玩家手牌的数组里面
      handPokers.push(
        handPoker.sort(function (a, b) {
          return b.slice(0, b.length - 1) - a.slice(0, a.length - 1);
        }),
      );
    }
    //返回所有玩家的手牌
    return handPokers;
  }

  //获取扑克牌的点数
  function getNum(str) {
    if (str.length == 2 || str.length == 3) {
      return str.slice(0, str.length - 1);
    } else {
      alert('扑克牌格式错误！');
    }
  }

  //获取扑克牌的花色
  function getColor(str) {
    if (str.length == 2 || str.length == 3) {
      return str.slice(-1);
    } else {
      alert('扑克牌格式错误！');
    }
  }
  //给玩家的手牌进行打分
  /*
	豹子：100000000000000
	顺金：1000000000000
	金花：10000000000
	顺子：100000000
	对子：1000000
	单牌：10000
	*/
  function points(playerPokers) {
    var playerPoker1 = playerPokers[0];
    var playerPoker2 = playerPokers[1];
    var playerPoker3 = playerPokers[2];
    if (
      getNum(playerPoker1) == getNum(playerPoker2) &&
      getNum(playerPoker2) == getNum(playerPoker3)
    ) {
      //豹子
      return 100000000000000 * getNum(playerPoker1);
    } else if (
      getNum(playerPoker1) - getNum(playerPoker2) == 1 &&
      getNum(playerPoker2) - getNum(playerPoker3) == 1
    ) {
      //首先肯定是个顺子
      if (
        getColor(playerPoker1) == getColor(playerPoker2) &&
        getColor(playerPoker2) == getColor(playerPoker1)
      ) {
        //顺金
        return 1000000000000 * getNum(playerPoker1);
      } else {
        //普通顺子
        return 100000000 * getNum(playerPoker1);
      }
    } else if (
      getColor(playerPoker1) == getColor(playerPoker2) &&
      getColor(playerPoker2) == getColor(playerPoker3)
    ) {
      //金花
      return (
        10000000000 * getNum(playerPoker1) +
        100000000 * getNum(playerPoker2) +
        1000000 * getNum(playerPoker3)
      );
    } else if (
      getNum(playerPoker1) == getNum(playerPoker2) ||
      getNum(playerPoker2) == getNum(playerPoker1)
    ) {
      //顺子
      if (getNum(playerPoker1) == getNum(playerPoker2)) {
        return 1000000 * getNum(playerPoker2) + 10000 * getNum(playerPoker3);
      } else {
        return 1000000 * getNum(playerPoker2) + 10000 * getNum(playerPoker1);
      }
    } else {
      return (
        10000 * getNum(playerPoker1) +
        100 * getNum(playerPoker2) +
        1 * getNum(playerPoker3)
      );
    }
  }

  //比较手牌的分数的大小
  function comparePoints(handPokers) {
    var allPoints = []; //所有玩家手牌的分数数组
    //遍历循环每个玩家的手牌
    for (var i = 0; i < handPokers.length; i++) {
      //调用points方法来计算玩家手牌的得分
      allPoints.push(points(handPokers[i]));
    }
    //求出哪个玩家的手牌得分最高
    var maxPoints = allPoints[0];
    for (var j = 0; j < allPoints.length; j++) {
      maxPoints = maxPoints > allPoints[j] ? maxPoints : allPoints[j];
    }
    // console.log(maxPoints);
    // 返回得分最高的玩家
    return allPoints.indexOf(maxPoints) + 1;
  }

  //生成最后需要输出的内容
  function mesPrint(winner, newHandPokers) {
    var alertMes = '';
    for (var i = 0; i < newHandPokers.length; i++) {
      alertMes =
        alertMes + '玩家' + (i + 1) + '的手牌为：' + newHandPokers[i] + '\n';
    }
    alertMes =
      alertMes +
      '---------------------------------------\n玩家' +
      winner +
      '的手牌最大，手牌为：' +
      newHandPokers[winner - 1] +
      '\n';
    return alertMes;
  }

  //扑克牌格式转换
  /*
	11 ---> J
	12 ---> Q
	13 ---> K
	14 ---> A
	*/
  function convertPoker(handPokers) {
    for (var i = 0; i < handPokers.length; i++) {
      for (var j = 0; j < handPokers[i].length; j++) {
        handPokers[i][j] = handPokers[i][j]
          .replace('11', 'J')
          .replace('12', 'Q')
          .replace('13', 'K')
          .replace('14', 'A');
      }
    }
    return handPokers;
  }

  return (
    <a
      onClick={() => {
        //定义玩家的数量
        var playerNum = 2;
        //调用getPorker方法，获取一副被打乱的扑克牌
        var poker = getPorker();
        //调用deal方法，给每个玩家发牌，获取所有玩家的手牌
        var handPokers = deal(playerNum, poker);
        //调用comparePoints方法，比较出手牌得分最高的玩家，获取到手牌得分最高的玩家
        //调用convertPoker方法，将扑克牌格式转换

        var winner = comparePoints(handPokers);
        var newHandPokers = convertPoker(handPokers);
        alert(mesPrint(winner, newHandPokers));
      }}
    >
      开始
    </a>
  );
};
```
