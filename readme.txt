package com.dafeiji.demo;
import java.awt.event.KeyAdapter;import java.awt.event.KeyEvent;
public class MoveListener extends KeyAdapter {
    private TanChiShe tanChiShe;        public MoveListener(TanChiShe tanChiShe) {        this.tanChiShe = tanChiShe;    }        @Override    public void keyReleased(KeyEvent e) {        int keyNum = e.getKeyCode();        if (!tanChiShe.isMoveFlg()) {            tanChiShe.setMoveFlg(true);        }        if (keyNum == 38 || keyNum == 87) {            // ↑ 又 W            tanChiShe.setMoveDirection(1);        } else if (keyNum == 37 || keyNum == 65) {            // ← 又 A            tanChiShe.setMoveDirection(2);        } else if (keyNum == 40 || keyNum == 83) {            // ↓ 又 S            tanChiShe.setMoveDirection(3);        } else if (keyNum == 39 || keyNum == 68) {            // → 又 D            tanChiShe.setMoveDirection(4);        }    }
}



package com.dafeiji.demo;
import java.util.ArrayList;import java.util.List;
public class TanChiShe {        private int moveDirection;            private List<TanChiSheWei> sheWeiLst = new ArrayList<>();        private boolean moveFlg;        private TanChiSheTou sheTou;        /**     * @return length     */    public int getLength() {        return sheWeiLst.size();    }
    /**     * @return moveDirection     */    public int getMoveDirection() {        return moveDirection;    }
    /**     * @param moveDirection 設定する moveDirection     */    public void setMoveDirection(int moveDirection) {        this.moveDirection = moveDirection;    }
    /**     * @return moveFlg     */    public boolean isMoveFlg() {        return moveFlg;    }
    /**     * @param moveFlg 設定する moveFlg     */    public void setMoveFlg(boolean moveFlg) {        this.moveFlg = moveFlg;    }
    /**     * @return sheWeiLst     */    public List<TanChiSheWei> getSheWeiLst() {        return sheWeiLst;    }
    /**     * @param sheWeiLst 設定する sheWei     */    public void addSheWeiLst(TanChiSheWei shemWei) {        this.sheWeiLst.add(shemWei);    }
    /**     * @return sheTou     */    public TanChiSheTou getSheTou() {        return sheTou;    }
    /**     * @param sheTou 設定する sheTou     */    public void setSheTou(TanChiSheTou sheTou) {        this.sheTou = sheTou;    }}
    
    
    
    package com.dafeiji.demo;
import java.awt.GridBagConstraints;import java.awt.GridBagLayout;import java.awt.Insets;import java.awt.Point;
import javax.swing.JFrame;import javax.swing.JPanel;import javax.swing.SwingUtilities;
public class TanChiSheFrame extends JFrame {    /**     *      */    private static final long serialVersionUID = 1L;    public TanChiSheFrame(){        setVisible(true);        setTitle("贪吃蛇");        setLocationRelativeTo(null);        setSize(500, 500);        setDefaultCloseOperation(DISPOSE_ON_CLOSE);        setLayout( new GridBagLayout() );        TanChiShe tanChiShe = new TanChiShe();        init(tanChiShe);        YouXiPan qiPan = new YouXiPan(tanChiShe);        addKeyListener(new MoveListener(tanChiShe));        add(new JPanel(), new GridBagConstraints(0, 0, 1, 1, 0.1, 0.1, GridBagConstraints.CENTER,                GridBagConstraints.BOTH, new Insets(0, 0, 0, 0), 0, 0));
        add(qiPan, new GridBagConstraints(0, 1, 1, 1, 0.8, 0.8, GridBagConstraints.CENTER, GridBagConstraints.NONE,                new Insets(0, 0, 0, 0), 0, 0));                add(new JPanel(), new GridBagConstraints(0, 2, 1, 1, 0.1, 0.1, GridBagConstraints.CENTER,                GridBagConstraints.NONE, new Insets(0, 0, 30, 0), 0, 0));    }        public void init(TanChiShe tanChiShe) {        TanChiSheTou shetou = new TanChiSheTou();        shetou.setPoint(new Point(8,8));        tanChiShe.setSheTou(shetou);                TanChiSheWei sheWei1 = new TanChiSheWei();        sheWei1.setPoint(new Point(16,8));                TanChiSheWei sheWei2 = new TanChiSheWei();        sheWei2.setPoint(new Point(24,8));        tanChiShe.addSheWeiLst(sheWei1);        tanChiShe.addSheWeiLst(sheWei2);    }    public static void main(String[] args) {        SwingUtilities.invokeLater(new Runnable() {            @Override            public void run() {                new TanChiSheFrame();            }        });    }}
        
        
        
 package com.dafeiji.demo;
import java.awt.Point;
public class TanChiSheTou {
    private Point point;
    /**     * @return point     */    public Point getPoint() {        return point;    }
    /**     * @param point 設定する point     */    public void setPoint(Point point) {        this.point = point;    }    }
    
    package com.dafeiji.demo;
import java.awt.Point;
public class TanChiSheWei {
    private Point point;
    /**     * @return point     */    public Point getPoint() {        return point;    }
    /**     * @param point 設定する point     */    public void setPoint(Point point) {        this.point = point;    }    }
    
    
package com.dafeiji.demo;
import java.awt.Color;import java.awt.Dimension;import java.awt.Graphics;import java.awt.Point;import java.awt.event.ActionEvent;import java.awt.event.ActionListener;import java.util.List;
import javax.swing.JPanel;import javax.swing.Timer;
public class YouXiPan extends JPanel {
    /**     *      */    private static final long serialVersionUID = 1L;
    private TanChiShe tanChiShe;        private boolean initFlg = true;        private boolean randomFlg = true;        private int randomXPoint;        private int randomYPoint;        private Timer timer;        private int delay = 100; // every 1 second        private int moveX = 0;        private int moveY = 0;        private int score;        public YouXiPan(TanChiShe tanChiShe) {        randomXPoint = (int) (Math.random() * 300 / 8) * 8;        randomYPoint = (int) (Math.random() * 300 / 8) * 8;        setBackground(Color.gray);        setPreferredSize(new Dimension(300, 300));        start();        this.tanChiShe = tanChiShe;        this.moveX = tanChiShe.getSheTou().getPoint().x;        this.moveY = tanChiShe.getSheTou().getPoint().y;    }
    @Override    public void paintComponent(Graphics g) {        super.paintComponent(g);        if (randomFlg) {            g.setColor(Color.WHITE);        } else {            g.setColor(Color.BLACK);        }        g.drawRect(randomXPoint, randomYPoint, 8, 8);        g.fillRect(randomXPoint, randomYPoint, 8, 8);        if(initFlg) {            initPaint(g);        }                if (tanChiShe.isMoveFlg()) {            initFlg = false;            movePaint(g);        }    }        private void initPaint(Graphics g) {        g.setColor(Color.BLACK);        g.drawRect(tanChiShe.getSheTou().getPoint().x, tanChiShe.getSheTou().getPoint().y, 8, 8);        g.setColor(Color.WHITE);        g.fillRect(tanChiShe.getSheTou().getPoint().x, tanChiShe.getSheTou().getPoint().y, 8, 8);        List<TanChiSheWei> sheWeiLst =  tanChiShe.getSheWeiLst();        for (TanChiSheWei sheWei : sheWeiLst) {            g.setColor(Color.BLACK);            g.drawRect(sheWei.getPoint().x, sheWei.getPoint().y, 8, 8);            g.setColor(Color.WHITE);            g.fillRect(sheWei.getPoint().x, sheWei.getPoint().y, 8, 8);        }    }        private void movePaint(Graphics g) {        if (tanChiShe.getMoveDirection() == 1) {            moveY = moveY - 8;        } else if (tanChiShe.getMoveDirection() == 2) {            moveX = moveX - 8;        } else if (tanChiShe.getMoveDirection() == 3) {            moveY = moveY + 8;        } else if (tanChiShe.getMoveDirection() == 4) {            moveX = moveX + 8;        }
        if (moveY <= 0 || moveX <= 0 || moveX >= 300 || moveY >= 300) {            end();            paintTanChiShe(g);            return;        }
        List<TanChiSheWei> sheWeiLst = tanChiShe.getSheWeiLst();        for (TanChiSheWei sheWei : sheWeiLst) {            int x = sheWei.getPoint().x;            int y = sheWei.getPoint().y;            if ((moveX == x - 8 && moveY == y) || (moveX == x && moveY == y - 8) || (moveX == x + 8 && moveY == y)                    || (moveX == x && moveY == y + 8)) {                end();                paintTanChiShe(g);                return;            }        }
        TanChiSheTou sheTou = tanChiShe.getSheTou();
        Point sheTouPoint = sheTou.getPoint();        int touX = sheTouPoint.x;        int touY = sheTouPoint.y;        Point nextShTiPoint = sheTou.getPoint();
        for (TanChiSheWei tanChiSheWei : sheWeiLst) {            nextShTiPoint = tanChiSheWei.getPoint();            tanChiSheWei.setPoint(sheTouPoint);            sheTouPoint = nextShTiPoint;        }
        sheTou.setPoint(new Point(moveX, moveY));        if (moveX == randomXPoint || moveY == randomYPoint) {            if ((touX == randomXPoint - 8 && touY == randomYPoint) || (touX == randomXPoint && touY == randomYPoint - 8)                    || (touX == randomXPoint + 8 && touY == randomYPoint)                    || (touX == randomXPoint && touY == randomYPoint + 8)) {                randomXPoint = (int) (Math.random() * 300 / 8) * 8;                randomYPoint = (int) (Math.random() * 300 / 8) * 8;                TanChiSheWei newSheWei = new TanChiSheWei();                newSheWei.setPoint(sheTouPoint);                tanChiShe.addSheWeiLst(newSheWei);            }        }
        paintTanChiShe(g);    }        private void paintTanChiShe(Graphics g) {        g.setColor(Color.BLACK);        g.drawRect(tanChiShe.getSheTou().getPoint().x, tanChiShe.getSheTou().getPoint().y, 8, 8);        g.setColor(Color.WHITE);        g.fillRect(tanChiShe.getSheTou().getPoint().x, tanChiShe.getSheTou().getPoint().y, 8, 8);
        for (TanChiSheWei sheWei : tanChiShe.getSheWeiLst()) {            g.setColor(Color.BLACK);            g.drawRect(sheWei.getPoint().x, sheWei.getPoint().y, 8, 8);            g.setColor(Color.WHITE);            g.fillRect(sheWei.getPoint().x, sheWei.getPoint().y, 8, 8);        }    }        private void start() {        ActionListener action = new ActionListener() {            @Override            public void actionPerformed(ActionEvent event) {                if (score > 1000) {                    end();                }                repaint();                if (randomFlg) {                    randomFlg = false;                } else {                    randomFlg  = true;                }                score ++;            }        };        timer = new Timer(delay, action);        timer.setInitialDelay(0);        timer.start();    }        private void end() {         timer.stop();    }}
