<script>
  import { onMount } from "svelte";
  import { v4 } from "uuid";
 

  let products = [];
    
  let product = {
    id: "",
    name: "",
    description: "",
    category: "",
    imageURL: ""
  };
  let editing = false;

  const onSubmit = e => {
    product.id = v4();
	products = products.concat(product);
	localStorage.setItem('product',JSON.stringify(products))
    resetForm();
  };

  const editProduct = id => {
    editing = true;
    product = products.find(p => p.id === id);
  };

  const resetForm = () => {
    product = {
      name: "",
      description: "",
      category: "",
      imageURL: ""
    };
  };

  const deleteProduct = id => {
	products = products.filter(p => p.id !== id);
	localStorage.setItem('product',JSON.stringify(products))
    
  };

  const updateProduct = e => {
	  e.preventDefault();
	products = products.map(p => (p.id === product.id ? { ...product } : p));
	
	editing = false;
	localStorage.setItem('product',JSON.stringify(products))
    e.target.reset();
  };

 
  onMount(()=>{
	let data = localStorage.getItem('product')
	if(data != null){
		products=JSON.parse(data)
	}
	
  })
  
</script>

<style>
  .edit {
    height: 70vh;
    overflow-y: scroll;
  }
</style>

<main>
  <div class="container mt-2">
    <div class="row">
      <div class="col-md-7 edit">
        {#each products as product}
          <div class="card">
            <div class="card-body">
              <div class="row">
                <div class="col-md-4">
                  <!-- <img src={product.imageURL ? product.imageURL : "./no-found.png"}  alt="" class="card-img-top" /> -->

                  {#if product.imageURL}
                    <img src={product.imageURL} alt="" class="card-img-top" />
                  {:else}
                    <img src="./no-found.png" alt="" class="card-img-top" />
                  {/if}
                </div>
                <div class="col-md-8 mt-5">
                  <ul>
                    <li>Name: {product.name}</li>
                    <li>description: {product.description}</li>
                    <li>Category: {product.category}</li>
                  </ul>
                </div>
              </div>
              <button
                class="btn btn-danger"
                on:click={deleteProduct(product.id)}>
                delete
              </button>
              <button class="btn btn-dark" on:click={editProduct(product.id)}>
                edit
              </button>
            </div>
          </div>
        {/each}
      </div>
      <div class="col-md-4">
        <div class="card">
          <div class="card-body">
            <form on:submit={editing ? updateProduct : onSubmit}>
              <div class="form-group">
                <input
                  bind:value={product.name}
                  type="text"
                  placeholder="name"
                  class="form-control" />
              </div>

              <div class="form-group">
                <textarea
                  bind:value={product.description}
                  id=""
                  rows="3"
                  placeholder="description"
                  class="form-control" />
              </div>

              <div class="form-group">
                <select
                  id=""
                  bind:value={product.category}
                  class="form-control">
                  <option>selecciona una categoria</option>
                  <option value="mouse">mouse</option>
                  <option value="keyboard">keyboard</option>
                  <option value="laptop">laptop</option>
                </select>
              </div>

              <div class="form-group">
                <input
                  bind:value={product.imageURL}
                  type="url"
                  placeholder="http;//example.com"
                  class="form-control" />
              </div>
             
                <button class="btn btn-dark btn-block">
				   {#if editing}edit product {:else} save product {/if}
				</button>
			 
            </form>
			
          </div>
        </div>
      </div>
    </div>
  </div>
</main>
