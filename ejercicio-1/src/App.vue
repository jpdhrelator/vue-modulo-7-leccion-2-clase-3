<template>
  <div>
    <h2>Encriptar</h2>
    <label>Ingresa tu clave</label>
    <input v-model="passwordDelUsuarioCreacion" type="text" >

    <br>
    <br>
    <label>Ingresa el texto a Encriptar</label> <br>
    <textarea v-model="textoSecreto"  cols="30" ></textarea>


     <br>
    <br>

    <button type="button" @click="crearSecreto()">Guardar</button>



    <hr>

    <h2>Desencriptar</h2>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">TExtoEncriptado</th>
          <th scope="col"></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="value in listadoEncriptado" :key="value.id" >
          <th scope="row">{{ value.id}}</th>
          <td>{{ value.nombre }}--->  {{ mostrarTextoCodificado(value.encriptado) }}</td>
          <td> <button @click="leerSecreto(value.id)" type="button" >Ver Valor</button></td>
        </tr>
      </tbody>
    </table>


    <br>
    <br>


       <label>Texto </label> <br>
      <textarea  v-model="textoSecretoDesencriptado" cols="30" ></textarea> <br>

      <label>Ingresa tu clave</label>
    <input v-model="passwordDelUsuarioLectura" type="text" ><br>
      
  </div>


</template>

<script setup>
import { ref } from 'vue';
const passwordDelUsuarioCreacion = ref('');
const passwordDelUsuarioLectura = ref('');
const listadoEncriptado=ref([]);
const textoSecreto= ref('');
const textoSecretoDesencriptado= ref('');

const salPublica= "texto-aleatorio-unico-por-usuario";


async function creaLLave(password, sal){
  const encoder = new TextEncoder();
   const materialBase = await window.crypto.subtle.importKey(
    "raw",
    encoder.encode(password),
    { name: "PBKDF2" },
    false,
    ["deriveKey"]
  );

  return await window.crypto.subtle.deriveKey(
    {
      name: "PBKDF2",
      salt: encoder.encode(sal),
      iterations: 100000, 
      hash: "SHA-256"
    },
    materialBase,
    { name: "AES-GCM", length: 256 },
    false, // La llave no se puede extraer ni leer con console.log
    ["encrypt", "decrypt"]
  );

}

async function crearSecreto(){
  if (!passwordDelUsuarioCreacion.value){
    return alert("Debes ingresar la clave")
  }

  const llaveTemporal = await creaLLave(passwordDelUsuarioCreacion.value,salPublica);

  const encoder = new TextEncoder();
  const iv = window.crypto.getRandomValues(new Uint8Array(12));



// AQUI OCURRE LA MAGIA!!!!
  const encriptado = await window.crypto.subtle.encrypt(
    { name: "AES-GCM", iv: iv },
    llaveTemporal, // Usamos la llave que acabamos de crear
    encoder.encode(textoSecreto.value)
  );




  listadoEncriptado.value.push({
    id:listadoEncriptado.value.length +1,
    nombre:`encritacion ${listadoEncriptado.value.length +1}`,
    encriptado,
    iv
  });
}

async function leerSecreto(id) {
 
   if (!passwordDelUsuarioLectura.value){
    return alert("Debes ingresar la clave")
  }
 try {
    const valor=listadoEncriptado.value.find((v)=> v.id==id);

    const textEncryp= valor.encriptado;
    const ivUsado=valor.iv

    const llaveTemporal = await creaLLave(passwordDelUsuarioLectura.value,salPublica);

    const desencriptado = await window.crypto.subtle.decrypt(
        { name: "AES-GCM", iv: ivUsado },
        llaveTemporal,
        textEncryp
    )
    const decoder = new TextDecoder();
    textoSecretoDesencriptado.value = decoder.decode(desencriptado);
  } catch (error) {

    console.error('Error al leer secreto',error);
    
  }
}

function mostrarTextoCodificado(codificado){
  const decoder = new TextDecoder();
  const textDeode=decoder.decode(codificado);
  console.log(textDeode);
  
  return textDeode;
}

</script>



<style scoped>

</style>


