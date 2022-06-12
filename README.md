# roller-rodo-o-rodillo
Script Camara
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
public class CameraC : MonoBehaviour
{
 public GameObject Personaje;
 private Vector3 offset;
 // Start is called before the first frame update
 void Start()
 {
 //distancia entre la cámara y el personaje
 offset = this.transform.position - Personaje.transform.position;
 }
 // Update is called once per frame
 void Update()
 {
 //conservar la distancia entre el jugador y la c+amara durante todo el juego
 this.transform.position = Personaje.transform.position + offset;
 }
}
Script Monedas
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
public class CoinBehavior : MonoBehaviour
{
//dará un efecto de rotación
 // Update is called once per frame
 void Update()
 {
 transform.Rotate(new Vector3 (15,30,45)*Time.deltaTime);
 
 }
}
Script Personaje
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
public class Personaje : MonoBehaviour
{
 public float fuerza;
 public GameObject CoinsContainer;
 private Vector3 dir;
 private Rigidbody rb;
 private int puntaje;
 private float tiempo;
 private bool isfirstClick, changeDir;
 private Text txtPuntaje, txtTiempo;
 //Start is called before the first frame update
 void Start()
 {
 //inicializacion de variables
 isfirstClick = false;
 changeDir = false;
 rb = GetComponent<Rigidbody>();
 dir = Vector3.zero;
 puntaje = 0; //puntaje inicial
 tiempo = 30; //tiempo disponible en segundos
 txtTiempo = GameObject.Find("Tiempo").GetComponent<Text>();
 txtPuntaje = GameObject.Find("Puntaje").GetComponent<Text>();
 }
 // Update is called once per frame
 void Update()
 {
 if (isfirstClick)
 {
 tiempo -= Time.deltaTime;
 }
 if (transform.position.y < -1)
 {
 restartGame();
 }
 //método para escuchar los clik del movimiento
 listenPlayerInteraction();
 }
 void FixedUpdate()
 {
 rb.MovePosition(transform.position + dir * Time.deltaTime * fuerza);
 }
 //método para controlar las colisiones
 void OnTriggerEnter(Collider obj)
 {
 if (obj.gameObject.tag == "Coin")
 {
 obj.gameObject.SetActive(false);
 puntaje++;
 txtPuntaje.text = "Puntaje: " + puntaje.ToString();
 }
 if (obj.gameObject.tag == "Meta") //ganar
 {
 //se reinicia el juego
 restartGame();
 }
 }
 //se crea el método
 void listenPlayerInteraction()
 {
 txtTiempo.text = "Tiempo: " + tiempo.ToString("F2");
 if (Input.GetMouseButtonDown(0))
 {
 if (!isfirstClick)
 {
 isfirstClick = true;
 }
 //parar bruscamente el movimeinto
 rb.Sleep();
 if (changeDir)
 {
 dir = new Vector3(-1, 0, 0); //movimiento a la izquierda
 changeDir = false;
 }
 else
 {
 dir = new Vector3(0, 0, 1); ///permite movimeinto adelante
 changeDir = true;
 }
 }
 }
 void restartGame()
 {
 Personaje objeto = this;
 puntaje = 0;
 txtPuntaje.text = "Puntaje: 0";
 tiempo = 30;
 txtTiempo.text = "Tiempo: 30";
 isfirstClick = false;
 changeDir = false;
 //regresar al personaje al inicio
 objeto.transform.localPosition = new Vector3(4.26f, -2.5f, -20.45f);
 rb.Sleep();
 dir = Vector3.zero;
 for (int i = 0; i < CoinsContainer.transform.childCount; i++)
 {
 CoinsContainer.transform.GetChild(i).gameObject.SetActive(true);
 }
 }
