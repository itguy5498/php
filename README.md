
<?php
class config{
    protected $server;
    protected $user;
    protected $password;
    protected $dbname;
    public $con;
    
    
    public function __construct($server,$user,$password,$dbname){
       
        $this->server=$server;
        $this->user=$user;
        $this->password=$password;
        $this->dbname=$dbname;
         $this->con=mysqli_connect($this->server,$this->user,$this->password,$this->dbname);
        if(!$this->con){
            echo "connection failed";
        }else{
            return $this->con;
        }
    }
      protected $fn;
        protected $em;
        protected $ps;
        protected $sql;
         private $check;
         private $query;
        public function  getdetails($fn,$em,$ps,$cps){
    
        $this->fn=mysqli_real_escape_string($this->con,$fn);
        $this->em=mysqli_real_escape_string($this->con,$em);
        $this->ps=mysqli_real_escape_string($this->con,$ps);
        
             if($this->fn=="" || $this->em=="" || $this->ps==""){
                 echo "<div class='col-lg-2'><div class='alert alert-danger' role='alert' id='hide' style='float: left'>Fill in the empty space!!!</div></div>";
             }else{
                 if($ps !== $cps){
                        echo "<div class='col-lg-2'><div class='alert alert-danger' role='alert' id='hide' style='float: left'>Password not equall !!!</div></div>";
                 }
             else{
                     $this->query=mysqli_query($this->con,"select * from register where email='$this->em' and fullname='$this->fn'");
                     
                     $this->check=mysqli_num_rows($this->query);
                     if($this->check){
                         echo "<div class='col-lg-2'><div class='alert alert-danger' role='alert' id='hide' style='float: left'>Email already exist!!!</div></div>";
                     }else{
                         $this->sql=mysqli_query($this->con,"insert into register (fullname,email,password) values('$this->fn','$this->em','$this->ps')");
                            if($this->sql){
                                header("location:pages-login-website.php");
                            }
                     }
                     
                 }
             }
    }
    public $qry;
    private $fun;
    private $ema;
    public $get_em;
    public function  login($em,$ps){
        $_SESSION['em']=$em;
        echo  $this->get_em=$_SESSION['em'];
            $this->qry=mysqli_query($this->con,"select*from register where email='$em' and password='$ps'");
           if(mysqli_fetch_array($this->qry)){
                header("location:index.php");
                while($row=mysqli_fetch_array($this->qry)){
                    $this->fun=$row['fullname'];
                    $_SESSION['fn']=$this->fun;
                    if($_SESSION['fn']){
                    echo   '<div class="profile-data-name">Trust Don</div>';
                    }
                }
                
            }else{
               echo  "<div class='alert alert-danger' role='alert' id='hide'>Incorect Password Or Ussername</div>";
           }
          
       
    }
  
    
    
    private $pa;private $us;
    public  function registered_user(){
        $this->sql=mysqli_query($this->con,"select*from register");
        while($row=$this->sql->fetch_array()){
            $this->fun=$row['fullname'];
            $this->ema=$row['email'];
            $this->pa=$row['password'];
            $this->us=$row['user_id'];
            
            
            

                                    echo        '<tbody>
                                                <tr id="trow_1">
                                                    <td class="text-center">'.$this->us.'</td>
                                                    <td><strong>'.ucfirst($this->fun).'</strong></td>
                                                    <td><span class="text-center">'.ucfirst($this->ema).'</span></td>
                                                    <td>'.ucfirst($this->pa).'</td>
                                          
                                                    <td>
                                                        <button  class="btn btn-default btn-rounded btn-sm popover-dismiss" title="Dismissible popover" data-placement="left"
                             data-content="<p>good</p>"><span class="fa fa-pencil"></span></button >
                                                        <button class="btn btn-danger btn-rounded btn-sm" ><span class="fa fa-times"></span></button>
                                                    </td>
                                                </tr>
                                               
                                            </tbody>';
                                        
        }
    }
    private $cat;
    public function add_category($cat){
        $this->sql=mysqli_query($this->con,"insert into add_category (category,creation_date) values('$cat',current_timestamp)");
        if($this->sql){
            echo "<div class='alert alert-success' role='alert' id='hide' style=' font-size: 15px; color:#000;' ><b>Inserted successfully !!</b></div>";
        }
    }
    private $date;
    private $cat_id;
    public function manage_category(){
        $this->sql=mysqli_query($this->con,"select*from add_category");
        while ($row=mysqli_fetch_array($this->sql)){
            $this->cat=$row['category'];
            $this->cat_id=$row['cat_id'];
            $this->date=$row['Creation_date'];
            
            echo '<tr>
<td>'.$this->cat_id.'</td>
<td>'.$this->cat.'</td>
<td>'.$this->date.'</td>
</tr>';
        }
    }
    
    private $itname;
    private $des;
    private $qu;
    private $pr;
    private $img_type;
    private $img_tmp;
    private $img_name;
    private $path;
    public function add_food($ca,$itname,$des,$qu,$pr,$type,$tmp,$name){
    $this->itname=mysqli_real_escape_string($this->con,$itname);
    $this->des=mysqli_real_escape_string($this->con,$des);
    $this->qu=mysqli_real_escape_string($this->con,$qu);
    $this->pr=mysqli_real_escape_string($this->con,$pr);
    $this->img_type=$type;
    $this->img_tmp=$tmp;
    $this->img_name=$name;
    $this->path="img2/$this->img_name";
    move_uploaded_file($tmp,$this->path);
    
        if(empty($itname ||$des||$qu||$pr)){
            echo   "<div class='alert alert-danger' role='alert' id='hide' style=' font-size: 15px; color:#000;' ><b>Fill the input space !!</b></div>";
        }
    else{
       $this->sql=mysqli_query($this->con,"insert into add_food (category,item_name,description,quantity,price,image)
     values('$ca','$this->itname','$this->des','$this->qu','$this->pr','$this->img_name')");
            if($this->sql){
                echo "<div class='alert alert-success' role='alert' id='hide' style=' font-size: 15px; color:#000;' ><b>Inserted successfully !!</b></div>";
            }
    }
    
    }
    public function getcate()
    {
        $this->sql = mysqli_query($this->con, "select*from add_category");
        while ($row = mysqli_fetch_array($this->sql)) {
            $this->cat = $row['category'];
            echo '
            <option value='.$this->cat.'>'.$this->cat.'</option>';
        }
    }

    private $item; 
    public $id;
   
 public function get_food_menu()
    {
        
        $this->sql = mysqli_query($this->con, "select*from add_food");
        while ($row = mysqli_fetch_array($this->sql)) {
            $this->cat = $row['category'];
            $this->id=$row['food_id'];
            $this->item=$row['item_name'];
            echo '<tbody>
            <tr >
            <td>'.$this->id.'</td>
            <td>'.$this->cat.'</td>
            <td>'.$this->item.'</td>
            <td>
            <a href="update_food_menu.php?food_id='.$this->id.'"><button class="btn btn-default btn-rounded btn-sm" name="edit"><span class="fa fa-pencil"></span></button></a>
                <button class="btn btn-danger btn-rounded btn-sm" onclick="delete_row("trow_1");" ><span class="fa fa-times"></span></button>
                </td>
        </tr>
        </tbody>';
        $_SESSION['id']=$this->id;
        
        $this->ids=$_SESSION['id'];
        }
     
    }
    public $ids;
    public function fetch_food_menu(){
    
        echo $this->ids;
        $this->sql = mysqli_query($this->con, "select*from add_food");
        while ($row = mysqli_fetch_array($this->sql)) {
            $this->cat = $row['category'];
            $this->id=$row['food_id'];
            $this->item=$row['item_name'];
        }
    }

    public function edit_food_menu($ca,$itname,$des,$qu,$pr,$type,$name){
    
     $this->sql=mysqli_query("update food_menu set category='$ca',item_name='$itname',description='$des', quantity='$qu',price='$pr',image='$name'");
     
    }

}

$dbconfig=new config("localhost","root","","restaurant");


if(isset($_POST['btn'])){
    $dbconfig->getdetails($_POST['fn'],$_POST['em'],$_POST['ps'],$_POST['cps']);
}if(isset($_POST['sub'])){
    $dbconfig->login($_POST['em'],$_POST['ps']);
    
}
if(isset($_POST['add'])){
    $dbconfig->add_category($_POST['cat']);
}
if(isset($_POST['btn2'])){
    
    $dbconfig->add_food($_POST['ca'],$_POST['itname'],$_POST['des'],$_POST['qu'],$_POST['pr'],$_FILES['img']['type'],$_FILES['img']['tmp_name'],$_FILES['img']['name']);
}














?>


